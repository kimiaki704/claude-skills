# ADR テンプレート

## フルテンプレート（大きな判断向け）

大きな判断（アーキテクチャ・フレームワーク選定等）に使う。必須セクション + オプションセクションをすべて含む。

```markdown
# NNNN: <タイトル>

- **Status**: Accepted
- **Date**: YYYY-MM-DD
- **Tags**: [architecture], [library], [ci-cd], [testing], [security], [process] 等

## Context

この判断が必要になった背景。何が起きていたか、何が問題だったか。
チーム規模・技術スタック・制約条件など、判断に影響した文脈を記述する。

## Decision

何を決めたか。1〜2 文で簡潔に。

## Rationale

なぜそう決めたか。判断の根拠を具体的に。
数値・事実・チームの合意事項を含めると再評価時に役立つ。

## Alternatives Considered

| 選択肢 | メリット | デメリット | 不採用の理由 |
|--------|---------|----------|------------|
| （選択肢 A） | ... | ... | ... |
| （選択肢 B） | ... | ... | ... |

## Consequences

この判断によって生じる影響。良い影響と悪い影響（トレードオフ）の両方を記述する。

## Revisit Trigger

以下の条件のいずれかが満たされた場合、この判断を再評価する:

- （条件 1。例: チーム規模が 5 名以上に拡大した場合）
- （条件 2。例: 採用技術のメンテナンスが停滞した場合）
- （条件 3。例: Apple が公式のアーキテクチャガイドラインを提供した場合）
```

---

## 軽量テンプレート（小さな判断向け）

小さな判断（ライブラリのバージョン固定、設定値の選択等）に使う。必須セクションのみ。

```markdown
# NNNN: <タイトル>

- **Status**: Accepted
- **Date**: YYYY-MM-DD
- **Tags**: [library], [config] 等

## Decision

何を決めたか。1〜2 文で。

## Rationale

なぜそう決めたか。

## Alternatives Considered

| 選択肢 | 不採用の理由 |
|--------|------------|
| （選択肢 A） | ... |

## Revisit Trigger

- （見直すべき条件）
```

---

## Status の値

| Status | 意味 |
|--------|------|
| `Proposed` | 検討中。まだ合意が取れていない |
| `Accepted` | 採用済み。現在も有効 |
| `Deprecated` | 廃止済み。後継の方針なし |
| `Superseded by NNNN` | 別の ADR（NNNN番）に取って代わられた |

---

## Tags の例

| Tag | 対象 |
|-----|------|
| `[architecture]` | アーキテクチャパターン（MVC, MVVM, Clean Arch 等） |
| `[library]` | ライブラリ・フレームワークの選定 |
| `[testing]` | テスト戦略・ツール |
| `[ci-cd]` | CI/CD パイプライン |
| `[security]` | 認証・認可・セキュリティ方針 |
| `[process]` | 開発プロセス・ワークフロー |
| `[config]` | 設定・環境変数・バージョン固定 |
| `[api]` | API 設計・通信方式 |

---

## 記入例

```markdown
# 0001: MVVM + Clean Architecture の採用

- **Status**: Accepted
- **Date**: 2026-01-15
- **Tags**: [architecture]

## Context

iOS 2名・Android 2名の小規模チーム。両プラットフォームで設計の概念を揃え、
レビューやコミュニケーションコストを削減したい。

## Decision

iOS アプリのアーキテクチャとして MVVM + Clean Architecture を採用する。

## Rationale

Domain / Data / Presentation の責務分離により、Android の Clean Architecture と
概念レベルで整合できる。チーム全員が設計の共通言語を持てる。

## Alternatives Considered

| 選択肢 | メリット | デメリット | 不採用の理由 |
|--------|---------|----------|------------|
| TCA | 状態管理の一貫性、テスタビリティ | PointFree 依存、学習コスト高 | サードパーティ依存リスク、Android 側に対応物がない |
| VIPER | 責務分離が明確 | ボイラープレートが多い | 小規模チームにはオーバーヘッド |

## Consequences

- (+) 設計レビューの共通語彙ができる
- (+) テスタビリティが向上する
- (-) ボイラープレートが増える
- (-) 新メンバーへの教育コストが発生する

## Revisit Trigger

- チーム規模が 5 名以上に拡大した場合
- Apple が公式のアーキテクチャガイドラインを提供した場合
- TCA が Apple 公式フレームワークに統合された場合
```
