# 深掘りタイプ定義

Phase 4（深掘り方向の選択）と Phase 5（深掘り調査）で使用する。
各タイプに対応する検索戦略とエビデンス評価の重点を定義する。

---

## タイプ一覧

### 1. 根拠補強

**ユーザーの意図**: 「この主張のエビデンスが弱い」「一次ソースが欲しい」

**検索戦略**:
- 主張のキーワード + `site:` で公式ドキュメント・論文リポジトリを指定
- 英語検索: `"{keyword}" official documentation` / `"{keyword}" RFC` / `"{keyword}" benchmark`
- 論文検索: `"{keyword}" site:arxiv.org` / `"{keyword}" site:scholar.google.com`
- 日本語補完: `"{keyword}" 公式ドキュメント` / `"{keyword}" 技術報告`

**エビデンス評価の重点**: E1-E2 のソースが見つかったかどうか。見つからない場合は「一次ソース未発見」としてギャップに残す。

**完了条件**: 対象の主張が E1-E2 に引き上げられる、または「一次ソースが存在しない」ことが確認される。

---

### 2. 代替探索

**ユーザーの意図**: 「他の選択肢は？」「A 以外にもある？」

**検索戦略**:
- 英語検索: `"{keyword}" alternatives` / `"{keyword}" vs` / `"{keyword}" comparison`
- 比較記事: `"{keyword}" vs "{alternative}" {current_year}`
- カテゴリ検索: `best {category} tools {current_year}` / `{category} framework comparison`
- 日本語補完: `"{keyword}" 比較` / `"{keyword}" 代替`

**エビデンス評価の重点**: 各代替案について同程度のエビデンスが集まっているか。片方だけ情報が豊富な比較は偏りがある。

**完了条件**: 主要な代替案が 2 つ以上特定され、それぞれについて E2 以上のソースがある。

---

### 3. 具体化

**ユーザーの意図**: 「実例が見たい」「実際に使っている事例は？」「ベンチマークは？」

**検索戦略**:
- ケーススタディ: `"{keyword}" case study` / `"{keyword}" in production` / `"{keyword}" at scale`
- ベンチマーク: `"{keyword}" benchmark {current_year}` / `"{keyword}" performance comparison`
- 実装例: `"{keyword}" example` / `"{keyword}" tutorial` / `"{keyword}" implementation`
- 企業事例: `"{keyword}" engineering blog` / `"{keyword}" postmortem`
- 日本語補完: `"{keyword}" 導入事例` / `"{keyword}" 本番運用`

**エビデンス評価の重点**: 事例の規模・再現性・文脈の類似性。小規模な個人プロジェクトの事例と、大規模プロダクションの事例を区別する。

**完了条件**: 具体的な事例が 1 つ以上見つかり、文脈（規模・用途・条件）が明記されている。

---

### 4. 反証検証

**ユーザーの意図**: 「本当にそうか？」「デメリットは？」「失敗した事例は？」

**検索戦略**:
- 反対意見: `"{keyword}" criticism` / `"{keyword}" drawbacks` / `"{keyword}" problems`
- 失敗事例: `"{keyword}" failure` / `"{keyword}" postmortem` / `"migrated away from {keyword}"`
- 制限事項: `"{keyword}" limitations` / `"{keyword}" not suitable for` / `"{keyword}" antipattern`
- 日本語補完: `"{keyword}" デメリット` / `"{keyword}" やめた` / `"{keyword}" 失敗`

**エビデンス評価の重点**: 反証の具体性。「遅い」のような抽象的な批判ではなく、条件付きの具体的な問題点を重視する。

**完了条件**: 少なくとも 1 つの具体的な反証・制限事項が E2 以上のソースで確認される、または「重大な反証は見つからなかった」ことが確認される。

---

### 5. 因果深掘り

**ユーザーの意図**: 「なぜそうなる？」「メカニズムは？」「根本原因は？」

**検索戦略**:
- メカニズム: `"{keyword}" how it works` / `"{keyword}" architecture` / `"{keyword}" internals`
- 原理: `"{keyword}" theory` / `"{keyword}" principle` / `"why {keyword} works"`
- 根本原因: `"{keyword}" root cause` / `"{keyword}" underlying reason`
- 論文: `"{keyword}" mechanism site:arxiv.org` / `"{keyword}" analysis`
- 日本語補完: `"{keyword}" 仕組み` / `"{keyword}" 原理` / `"{keyword}" なぜ`

**エビデンス評価の重点**: 因果関係の説明が論理的に一貫しているか。相関と因果を混同していないか。

**完了条件**: 因果メカニズムが E1-E2 のソースで説明される、または「メカニズムは未解明」であることが確認される。

---

### 6. シナリオ分岐

**ユーザーの意図**: 「この先どうなる？」「未来のシナリオを考えて」「どんな可能性がある？」

**検索戦略**:
- トレンド: `"{keyword}" trend {current_year}` / `"{keyword}" forecast` / `"{keyword}" prediction`
- 業界レポート: `"{keyword}" market report` / `"{keyword}" outlook` / `"state of {keyword} {current_year}"`
- 専門家の見解: `"{keyword}" future` / `"{keyword}" roadmap` / `"{keyword}" next`
- 阻害要因: `"{keyword}" challenges` / `"{keyword}" barriers` / `"{keyword}" risks`
- 日本語補完: `"{keyword}" 動向` / `"{keyword}" 将来` / `"{keyword}" ロードマップ`

**エビデンス評価の重点**: トレンドの根拠が E1-E2 レベルであること。推論（P レベル）と事実（E レベル）を明確に分離する。

**完了条件**: 少なくとも 2 つのシナリオが P1-P3 で生成される。P4（思考実験）のみの場合は「根拠が不十分」と明示する。

**シナリオ生成の詳細**: `references/scenario-framework.md` を参照。

---

## Phase 4 での推奨ロジック

ギャップマップの各項目に対して、以下の優先順位で深掘りタイプを推奨する:

| ギャップの種類 | 第一推奨 | 第二推奨 |
|-------------|---------|---------|
| E4 が多い領域 | 根拠補強 | 因果深掘り |
| 単一ソース依存 | 根拠補強 | 反証検証 |
| 代替案が未調査 | 代替探索 | 具体化 |
| 反対意見が未調査 | 反証検証 | 具体化 |
| 因果関係が不明 | 因果深掘り | 根拠補強 |
| 具体例が不足 | 具体化 | 代替探索 |
| 矛盾がある | 反証検証 | 根拠補強 |
| ユーザーが「この先」と発言 | シナリオ分岐 | 因果深掘り |
