# claude-skills

AI 駆動開発のためのスキルプラグイン。ドキュメント・ワークフローの業界水準レビュー、タスクの構造化・issue 起票、LT・プレゼンの構成案を対話的に作成、そしてプロジェクトコンテキストと紐づけた技術動向のインパクト分析。

> **English**: Claude Code plugin providing four skills for AI-driven development: (1) **deep-review** — multi-perspective gap analysis against industry best practices, (2) **task-decompose** — structured task decomposition and GitHub issue creation, (3) **lt-outline-builder** — interactive wizard for creating LT/presentation outlines, and (4) **context-briefing** — project-aware tech trend briefing with impact analysis.

## Install

```bash
# Plugin marketplace 経由（推奨）
/plugin marketplace add kimiaki704/claude-skills
/plugin install claude-skills@claude-skills

# または手動コピー
git clone https://github.com/kimiaki704/claude-skills.git
cp -r claude-skills/skills/deep-review ~/.claude/skills/
cp -r claude-skills/skills/task-decompose ~/.claude/skills/
cp -r claude-skills/skills/lt-outline-builder ~/.claude/skills/
cp -r claude-skills/skills/context-briefing ~/.claude/skills/
```

## Skills

| Skill | Description | Recommended Model |
|-------|-------------|-------------------|
| **[deep-review](skills/deep-review/SKILL.md)** | ドキュメント・ワークフロー・設定ファイルを業界ベストプラクティスと複数専門家視点で分析し、改善提案を優先度付きで提示する。Quick モード（Sonnet）と Full モード（Opus）を選択可能 | Quick: Sonnet / Full: Opus |
| **[task-decompose](skills/task-decompose/SKILL.md)** | 抽象的なタスクを壁打ちで構造化し、「なぜ・何を・いつまでに」が明確な issue として起票する。タスク分類・INVEST チェック・質問ループ制御を内蔵 | Opus（曖昧な入力）/ Sonnet（具体的な入力） |
| **[lt-outline-builder](skills/lt-outline-builder/SKILL.md)** | LT・プレゼンの構成案を4段階の対話ウィザードで作成する。発表条件の把握→コアメッセージ抽出→フレームワーク自動選択→スライド案出力まで一気通貫 | Sonnet |
| **[context-briefing](skills/context-briefing/SKILL.md)** | プロジェクトコンテキスト（技術スタック・依存ライブラリ）を読み込み、指定した期間×ジャンルの技術動向をインパクト分析付きで返す定点観測スキル。「うちに関係ある変化は何か」を答える | Quick: Sonnet / Full: Opus |

### deep-review

7 Phase のメタレビューワークフロー:

1. **対象の理解** — ファイル通読 + 構造要約
2. **業界調査** — WebSearch で 3+ ソースを調査（Full のみ）
3. **マルチパースペクティブ** — 3-5 ペルソナで並列レビュー（Full のみ）
4. **ギャップ分析** — 欠落 / 乖離 / 過剰 / 強みを特定
5. **改善提案** — 優先度 + 選択肢比較テーブル付き
6. **ユーザー合意** — 採否を確認
7. **実装 + レビューループ** — セルフレビュー + 外部 AI レビュー（Full のみ）

### task-decompose

6 Step の壁打ちフロー:

1. **タスク受け取り** — 分類（調査/実装/バグ/リファクタ/設計・意思決定）+ ショートカット判定
2. **背景の深掘り** — なぜ必要か、何に影響するか
3. **ゴール具体化** — 成果物・完了条件・スコープ（質問は合計 2 回まで）
4. **粒度チェック** — 分割判定 + INVEST 簡易版チェック
5. **期日設定** — 根拠と中間確認ポイント
6. **テンプレート出力** — テキスト / md ファイル / `gh issue create` の 3 段階

### lt-outline-builder

4 Stage の対話ウィザード:

1. **発表条件と聴衆の把握** — 時間・形式・聴衆属性を雑談ベースで確認
2. **テーマ深掘りとコアメッセージ抽出** — 1文で表現できるキーメッセージに収束（最大2つ）
3. **フレームワーク選択と構成案生成** — PREP / 結起承転結 / SCR / TAPS 等8種から自動提案 + フィードバックループ
4. **スライド案と時間配分の提示** — スライドタイトル・キーメッセージ・推奨ビジュアル・想定時間を一覧出力

### context-briefing

6 Phase の定点観測ワークフロー:

1. **入力収集** — 期間 × ジャンルを確認。モード（Quick/Full）を推定・提案
2. **プロジェクトコンテキスト取り込み** — AGENTS.md / Package.swift 等を読み込みサマリーを作成
3. **情報収集** — WebSearch でジャンル別に 2〜3 回検索（英語 + 日本語）
4. **インパクト分析** — 収集情報とプロジェクトコンテキストを突合。🔴/🟡/🟢/⚪ で影響度判定
5. **ブリーフィング出力** — 「何が起きたか・うちへの影響・推奨アクション」の3点セット
6. **task-decompose 連携** — 🔴/🟡 トピックの issue 化をワンクリックで引き継ぎ

## Compatibility

| Tool | Support |
|------|---------|
| Claude Code | Plugin install / manual copy |
| Codex CLI | Manual copy to project skills |
| Cursor | Manual copy to project skills |
| Other SKILL.md-compatible tools | Manual copy |

## Structure

```
claude-skills/
├── .claude-plugin/
│   ├── plugin.json          # Plugin metadata
│   └── marketplace.json     # Marketplace catalog
├── skills/
│   ├── deep-review/
│   │   └── SKILL.md         # 7-phase meta-review workflow
│   ├── task-decompose/
│   │   ├── SKILL.md         # 6-step task decomposition
│   │   └── references/
│   │       └── template.md  # Issue template (Progressive Disclosure)
│   ├── lt-outline-builder/
│   │   ├── SKILL.md         # 4-stage interactive outline wizard
│   │   └── references/
│   │       ├── frameworks.md        # 8 presentation frameworks
│   │       └── slide-guidelines.md  # Slide count & timing guidelines
│   └── context-briefing/
│       ├── SKILL.md         # 6-phase project-aware briefing workflow
│       └── references/
│           ├── impact-framework.md      # Impact level definitions & evaluation axes
│           └── genre-search-patterns.md # Search query patterns by genre
├── README.md
├── LICENSE                  # MIT
└── CONTRIBUTING.md
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)
