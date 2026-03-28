---
name: add-skill
description: >
  このリポジトリに新しいスキルを追加する。skill-creator の設計手法に基づいて
  worktree 作成 → スキル実装 → プラグイン更新 → Draft PR 作成まで一気通貫で行う。
  「新しいスキルを作りたい」「スキルを追加して」「〇〇スキルを実装して」
  「スペックを渡すのでスキルにして」といった場面で使う。
argument-hint: "[スキル名またはスペックファイルのパス]"
disable-model-invocation: true
---

# add-skill — このリポジトリへのスキル追加ワークフロー

skill-creator の5ステップ手法に従い、worktree の作成から Draft PR の作成まで一気通貫で実行する。

## リポジトリの慣習

| 要素 | 形式 | 例 |
|------|------|---|
| ブランチ名 | `feat/[skill-name]` | `feat/lt-outline-builder` |
| worktree パス | `../claude-skills-[skill-name]` | `../claude-skills-lt-outline-builder` |
| スキルディレクトリ | `skills/[skill-name]/` | `skills/lt-outline-builder/` |
| バージョン | `1.x.0`（マイナー+1） | `1.0.0` → `1.1.0` |
| PRアカウント | `gh pr create` 実行時のログイン中アカウント | — |

---

## Step 1: スキル仕様の確認

引数でスペックファイルが渡された場合は Read で読み込む。渡されていない場合は以下を AskUserQuestion で確認する:

- **スキル名**（kebab-case、64文字以内）
- **どんな場面で使うか**（具体的な使用例を 2〜3 個）
- **このスキルがやること / やらないこと**
- **references/ に格納すべき知識はあるか**（フレームワーク一覧、ガイドライン等）

**skill-creator の鉄則**: description はやや積極的（pushy）に書く。Claude はスキルをアンダートリガーする傾向があるため、トリガーすべき場面を具体的に列挙する。

---

## Step 1.5: ドラフト承認

実装に入る前に、以下をユーザーに提示して AskUserQuestion で承認を得る:

- **スキル名**（kebab-case）
- **description**（pushy 版・最終案）
- **想定ユースケース 3件**
- **このスキルがやらないこと**

承認されたら Step 2 へ進む。修正が出た場合は内容を更新して再提示する。

---

## Step 2: worktree と ディレクトリ構造の作成

```bash
# worktree 作成
git worktree add ../claude-skills-[skill-name] -b feat/[skill-name]

# ディレクトリ構造
mkdir -p ../claude-skills-[skill-name]/skills/[skill-name]/references
```

作成後、worktree パスを確認:
```bash
git worktree list
```

---

## Step 3: SKILL.md の実装

worktree 内の `skills/[skill-name]/SKILL.md` を作成する。

### SKILL.md の必須要素

```yaml
---
name: [skill-name]           # kebab-case
description: >               # やや積極的に書く。トリガー条件を列挙
  〇〇するスキル。〇〇、〇〇、〇〇が必要なときに使う。
  「〜したい」「〜して」といった場面でも使う。
license: MIT
# --- オプションフィールド ---
argument-hint: "[引数のヒント]"       # スラッシュコマンドの補完ヒント（任意）
disable-model-invocation: true        # スキル呼び出し時に Claude の通常推論を無効化し、
                                      # SKILL.md の手順書として完全に動かしたい場合に指定（任意）
---
```

### 設計チェックリスト（実装時のセルフチェック）

実装完了後に以下を自己確認する。これは **形式・構造** の確認であり、設計方針の検証は Step 6 の deep-review で行う。

- [ ] SKILL.md 本体は **500行以内**
- [ ] Claude がすでに知っていることは書かない（スキル固有の知識のみ）
- [ ] 詳細な参照資料は `references/` に切り出す
- [ ] フォールバックを定義する（よくある失敗ケースの対処）
- [ ] 「このスキルがやらないこと」を明記する

### 対話型スキルの場合（ウィザード形式）

- 各 Stage の完了条件で **AskUserQuestion** を使う（キーワードマッチ不可）
- Stage 完了時に **引き継ぎサマリー** を出力して次 Stage に情報を渡す
- フィードバックループのラウンド上限はユーザー承認ベースにする

### references/ に格納すべきもの

- フレームワーク・手法の詳細一覧
- 時間別・目的別のガイドライン
- テンプレート
- ペルソナカタログ

---

## Step 4: プラグインファイルの更新

worktree 内の以下を更新する。完了したものをチェックしてから次に進む。

### `.claude-plugin/plugin.json`

```json
{
  "version": "1.x.0",           // マイナーバージョンを +1
  "keywords": ["...", "[skill-name]"]  // 新スキル名を追加
}
```

### `.claude-plugin/marketplace.json`

```json
{
  "metadata": { "version": "1.x.0" },
  "plugins": [{
    "version": "1.x.0",
    "description": "...[新スキルの説明を追記]...",
    "keywords": ["...", "[skill-name]"]
  }]
}
```

### `README.md` 更新チェックリスト

以下の4箇所をすべて更新してからコミットへ進む:

- [ ] **冒頭の説明文**（日本語・英語）に新スキルを追記
- [ ] **Skills テーブル**に新スキルの行を追加
- [ ] **Install セクション**に `cp -r` コマンドを追記
- [ ] **Structure セクション**のディレクトリツリーを更新

---

## Step 5: コミット・プッシュ・PR 作成

```bash
# worktree 内でコミット
git -C ../claude-skills-[skill-name] add skills/[skill-name]/ \
  .claude-plugin/marketplace.json .claude-plugin/plugin.json README.md
git -C ../claude-skills-[skill-name] commit -m "feat: [skill-name] スキルを追加"

# プッシュ
git -C ../claude-skills-[skill-name] push -u origin feat/[skill-name]

# PR 作成
cd ../claude-skills-[skill-name] && gh pr create --draft \
  --title "feat: [skill-name] スキルを追加" \
  --body "[PR本文]"
```

### PR 本文の構成

```markdown
## Summary
- [スキルの概要・追加理由]

## 追加ファイル
| ファイル | 内容 |
...

## 変更ファイル
- README.md / plugin.json / marketplace.json の更新内容

## Test plan
- [ ] `/[skill-name]` でスキルが起動する
- [ ] [主要なトリガーフレーズ] で自動トリガーされる
- [ ] フォールバックが機能する
- [ ] テスト方法: Claude Code 上で `/[skill-name]` を実行し、AskUserQuestion が表示されることを確認する
```

---

## Step 6: deep-review の実施確認

PR 作成後、AskUserQuestion でユーザーに確認する:

> 「PR を作成しました。このスキルの設計を `/deep-review` で検証しますか？」
> - **実行する** → Quick モードで `/deep-review` を起動（description のトリガー精度・フォールバック網羅性・AskUserQuestion の一貫性を検証）
> - **スキップする** → ワークフロー完了

deep-review では **設計方針** を検証する（Step 3 チェックリストの形式確認とは役割が異なる）:
- description のトリガー精度
- Progressive Disclosure の設計判断
- フォールバックの網羅性
- AskUserQuestion の一貫性

---

## Step 7: worktree のクリーンアップ（PR マージ後）

PR がマージまたはクローズされたら、worktree を削除する:

```bash
git worktree remove ../claude-skills-[skill-name]
git branch -d feat/[skill-name]
```

> **注**: マージ前に削除すると変更が失われる。PR の状態を確認してから実行すること。

---

## フォールバック

| 状況 | 対処 |
|------|------|
| スキル名が未定 | 用途・目的から kebab-case で提案して確認する |
| worktree が既に存在する | `git worktree list` で確認し、既存 worktree を使うか削除するか聞く |
| スペックが不明確 | Step 1 の AskUserQuestion を繰り返して仕様を固める |
| ドラフト承認で修正が出た | Step 1.5 を繰り返して合意を得てから Step 2 へ進む |
