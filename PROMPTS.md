# 実験プロンプト一覧

論文の実験で使用した英語プロンプトのテンプレート全文を記載します。テンプレート中の `{S}` には主語の Wikidata 英語ラベルが、`{Y}` には事実ごとに定めた過去の年（[DATA.md](DATA.md) の 3 節）が入ります。いずれのテンプレートもモデルの出力を確認する前に設計・固定しました。

同一内容を機械可読形式でまとめた JSON ファイルを `prompts/` 以下に配置しています。

- `prompts/templates_en.json`：1 節の評価用テンプレート
- `prompts/gate_templates_en.json`：2 節の時間ゲート学習用テンプレート（英語）
- `prompts/gate_templates_ja.json`：2 節の時間ゲート学習用テンプレート（日本語）

質問形式のテンプレートは原則として「質問文（回答形式の指示）」「改行」「`Answer:`」の順で構成されています。文の続きを補完させる形式（`apply_eval` の 2 文目）および同一主語に関する無関係な質問（`negative`）には回答形式の指示を付けていません。モデルへの入力時には、プロンプト先頭に BOS トークンを付与します。

関係ごとに用意した 8 種類のテンプレートは以下のとおりです。

| キー名 | 文数 | 内容 | 用途 |
|---|---:|---|---|
| `method_q` | 1 | 時点無指定の質問 | 提案手法・比較手法に与える質問 $q$、対象事実の選定、忘却の評価 |
| `apply_eval` | 4 | 時点無指定質問の言い換え（2 文目は文補完形式、4 文目は `Currently` で始まる形式） | 対象事実の選定、忘却の評価 |
| `method_qref` | 1 | 2023 年末を指定した質問 | 提案手法に与える基準時点質問 $q_{\mathrm{ref}}$ |
| `reference_eval` | 3 | 2023 年末を指定した質問（`method_qref` とは異なる言い換え） | 対象事実の選定 |
| `retain_eval` | 3 | 過去の年 `{Y}` を質問文中に含む質問（1・2 文目は文頭、3 文目は文末配置） | 過去時点の保持の評価 |
| `retain_pos` | 1 | 過去の年 `{Y}` を括弧内の指示に含む質問 | 過去時点の保持の評価 |
| `retain_relative` | 1 | 年を明示せず前の値を尋ねる質問 | 過去時点の保持の評価 |
| `negative` | 2 | 同一主語についての無関係な質問（人物・組織・場所の種別ごとに作成） | 無関係知識の保持の評価 |

論文中の集合 $U_f$ は `method_q` と `apply_eval` を合わせた 5 文、$K_f$ は `reference_eval` の 3 文に対応します。無関係知識の保持の評価には、`negative` の 2 文に加えて他事実の `method_q` を 3 文使用します（[DATA.md](DATA.md) の 7 節）。

## 1. 評価用プロンプトのテンプレート

### P6 head of government（行政府の長）

時点無指定質問（手法に与える q）（`method_q`）

```text
Who is the head of government of {S}? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the head of government of {S}. (Answer with the name only.)
Answer:

The head of government of {S} is

Can you name the head of government of {S}? (Answer with the name only.)
Answer:

Currently, who is the head of government of {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, who was the head of government of {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, who was the head of government of {S}? (Answer with the name only.)
Answer:

Who was the head of government of {S} on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, who was the head of government of {S}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, who was the head of government of {S}? (Answer with the name only.)
Answer:

Back in {Y}, who was the head of government of {S}? (Answer with the name only.)
Answer:

Who was the head of government of {S} in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
Who was the head of government of {S}? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
Who was the previous head of government of {S}? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where is {S} located?
Answer:

What is the history of {S}?
Answer:
```

### P35 head of state（国家元首）

時点無指定質問（手法に与える q）（`method_q`）

```text
Who is the head of state of {S}? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the head of state of {S}. (Answer with the name only.)
Answer:

The head of state of {S} is

Can you name the head of state of {S}? (Answer with the name only.)
Answer:

Currently, who is the head of state of {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, who was the head of state of {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, who was the head of state of {S}? (Answer with the name only.)
Answer:

Who was the head of state of {S} on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, who was the head of state of {S}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, who was the head of state of {S}? (Answer with the name only.)
Answer:

Back in {Y}, who was the head of state of {S}? (Answer with the name only.)
Answer:

Who was the head of state of {S} in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
Who was the head of state of {S}? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
Who was the previous head of state of {S}? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where is {S} located?
Answer:

What is the history of {S}?
Answer:
```

### P488 chairperson（議長）

時点無指定質問（手法に与える q）（`method_q`）

```text
Who is the chair of {S}? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the chair of {S}. (Answer with the name only.)
Answer:

The chair of {S} is

Can you name the chair of {S}? (Answer with the name only.)
Answer:

Currently, who is the chair of {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, who was the chair of {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, who was the chair of {S}? (Answer with the name only.)
Answer:

Who was the chair of {S} on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, who was the chair of {S}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, who was the chair of {S}? (Answer with the name only.)
Answer:

Back in {Y}, who was the chair of {S}? (Answer with the name only.)
Answer:

Who was the chair of {S} in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
Who was the chair of {S}? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
Who was the previous chair of {S}? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where is {S} based?
Answer:

When was {S} founded?
Answer:
```

### P286 head coach（監督）

時点無指定質問（手法に与える q）（`method_q`）

```text
Who is the head coach of {S}? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the head coach of {S}. (Answer with the name only.)
Answer:

The head coach of {S} is

Can you name the head coach of {S}? (Answer with the name only.)
Answer:

Currently, who is the head coach of {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, who was the head coach of {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, who was the head coach of {S}? (Answer with the name only.)
Answer:

Who was the head coach of {S} on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, who was the head coach of {S}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, who was the head coach of {S}? (Answer with the name only.)
Answer:

Back in {Y}, who was the head coach of {S}? (Answer with the name only.)
Answer:

Who was the head coach of {S} in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
Who was the head coach of {S}? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
Who was the previous head coach of {S}? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where is {S} based?
Answer:

When was {S} founded?
Answer:
```

### P169 chief executive officer（最高経営責任者）

時点無指定質問（手法に与える q）（`method_q`）

```text
Who is the chief executive officer of {S}? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the chief executive officer of {S}. (Answer with the name only.)
Answer:

The chief executive officer of {S} is

Can you name the chief executive officer of {S}? (Answer with the name only.)
Answer:

Currently, who is the chief executive officer of {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, who was the chief executive officer of {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, who was the chief executive officer of {S}? (Answer with the name only.)
Answer:

Who was the chief executive officer of {S} on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, who was the chief executive officer of {S}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, who was the chief executive officer of {S}? (Answer with the name only.)
Answer:

Back in {Y}, who was the chief executive officer of {S}? (Answer with the name only.)
Answer:

Who was the chief executive officer of {S} in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
Who was the chief executive officer of {S}? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
Who was the previous chief executive officer of {S}? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where is {S} based?
Answer:

When was {S} founded?
Answer:
```

### P127 owned by（所有者）

時点無指定質問（手法に与える q）（`method_q`）

```text
Who owns {S}? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the owner of {S}. (Answer with the name only.)
Answer:

{S} is owned by

Can you name the owner of {S}? (Answer with the name only.)
Answer:

Currently, who owns {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, who owned {S}? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, who owned {S}? (Answer with the name only.)
Answer:

Who owned {S} on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, who owned {S}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, who owned {S}? (Answer with the name only.)
Answer:

Back in {Y}, who owned {S}? (Answer with the name only.)
Answer:

Who owned {S} in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
Who owned {S}? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
Who owned {S} before its current owner? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where is {S} based?
Answer:

When was {S} founded?
Answer:
```

### P54 member of sports team（所属チーム）

時点無指定質問（手法に与える q）（`method_q`）

```text
Which team does {S} play for? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the team that {S} plays for. (Answer with the name only.)
Answer:

{S} plays for

Can you name the team that {S} plays for? (Answer with the name only.)
Answer:

Currently, which team does {S} play for? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, which team did {S} play for? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, which team did {S} play for? (Answer with the name only.)
Answer:

Which team did {S} play for on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, which team did {S} play for? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, which team did {S} play for? (Answer with the name only.)
Answer:

Back in {Y}, which team did {S} play for? (Answer with the name only.)
Answer:

Which team did {S} play for in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
Which team did {S} play for? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
Which team did {S} play for before joining their current team? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where was {S} born?
Answer:

What is {S} known for?
Answer:
```

### P39 position held（役職）

時点無指定質問（手法に与える q）（`method_q`）

```text
What position does {S} hold? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the position that {S} holds. (Answer with the name only.)
Answer:

{S} holds the position of

Can you name the position that {S} holds? (Answer with the name only.)
Answer:

Currently, what position does {S} hold? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, what position did {S} hold? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, what position did {S} hold? (Answer with the name only.)
Answer:

What position did {S} hold on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, what position did {S} hold? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, what position did {S} hold? (Answer with the name only.)
Answer:

Back in {Y}, what position did {S} hold? (Answer with the name only.)
Answer:

What position did {S} hold in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
What position did {S} hold? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
What position did {S} hold before their current one? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where was {S} born?
Answer:

What is {S} known for?
Answer:
```

### P108 employer（雇用主）

時点無指定質問（手法に与える q）（`method_q`）

```text
Which organization does {S} work for? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the organization that {S} works for. (Answer with the name only.)
Answer:

{S} works for

Can you name the organization that {S} works for? (Answer with the name only.)
Answer:

Currently, which organization does {S} work for? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, which organization did {S} work for? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, which organization did {S} work for? (Answer with the name only.)
Answer:

Which organization did {S} work for on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, which organization did {S} work for? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, which organization did {S} work for? (Answer with the name only.)
Answer:

Back in {Y}, which organization did {S} work for? (Answer with the name only.)
Answer:

Which organization did {S} work for in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
Which organization did {S} work for? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
Which organization did {S} work for before their current employer? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where was {S} born?
Answer:

What is {S} known for?
Answer:
```

### P102 member of political party（所属政党）

時点無指定質問（手法に与える q）（`method_q`）

```text
Which political party is {S} a member of? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the political party that {S} belongs to. (Answer with the name only.)
Answer:

{S} is a member of the

Can you name the political party that {S} belongs to? (Answer with the name only.)
Answer:

Currently, which political party is {S} a member of? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, which political party was {S} a member of? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, which political party was {S} a member of? (Answer with the name only.)
Answer:

Which political party was {S} a member of on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, which political party was {S} a member of? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, which political party was {S} a member of? (Answer with the name only.)
Answer:

Back in {Y}, which political party was {S} a member of? (Answer with the name only.)
Answer:

Which political party was {S} a member of in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
Which political party was {S} a member of? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
Which political party was {S} a member of before their current party? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where was {S} born?
Answer:

What is {S} known for?
Answer:
```

### P69 educated at（出身校）

時点無指定質問（手法に与える q）（`method_q`）

```text
Which school does {S} attend? (Answer with the name only.)
Answer:
```

時点無指定質問の言い換え 4 文（`apply_eval`）

```text
Tell me the school that {S} attends. (Answer with the name only.)
Answer:

{S} attends

Can you name the school that {S} attends? (Answer with the name only.)
Answer:

Currently, which school does {S} attend? (Answer with the name only.)
Answer:
```

2023 年末指定質問（手法に与える q_ref）（`method_qref`）

```text
As of the end of 2023, which school did {S} attend? (Answer with the name only.)
Answer:
```

2023 年末指定質問 3 文（`reference_eval`）

```text
At the end of 2023, which school did {S} attend? (Answer with the name only.)
Answer:

Which school did {S} attend on December 31, 2023? (Answer with the name only.)
Answer:

In December 2023, which school did {S} attend? (Answer with the name only.)
Answer:
```

過去の年 {Y} を質問文に含む質問 3 文（`retain_eval`）

```text
As of {Y}, which school did {S} attend? (Answer with the name only.)
Answer:

Back in {Y}, which school did {S} attend? (Answer with the name only.)
Answer:

Which school did {S} attend in {Y}? (Answer with the name only.)
Answer:
```

過去の年 {Y} を括弧内の指示に含む質問（`retain_pos`）

```text
Which school did {S} attend? (Use information as of {Y}; answer with the name only.)
Answer:
```

年を明示せず前の値を尋ねる質問（`retain_relative`）

```text
Which school did {S} attend before their current school? (Answer with the name only.)
Answer:
```

同一主語に関する無関係な質問 2 文（`negative`）

```text
Where was {S} born?
Answer:

What is {S} known for?
Answer:
```

## 2. 時間ゲートの学習用固定文テンプレート

時間ゲートの学習には、手法に与えた事実の質問に加えて、あらかじめ用意した固定文を使用します。固定文は開発分割（`dev`）に含まれる事実の主語から作成しました（主語の選定方法は [DATA.md](DATA.md) の 8 節を参照）。主語ごとに過去時点を指定した文を 5 文、時点無指定の文を 5 文生成しています（`{Y}` にはその主語の事実がもつ過去の年が入り、言い回しは 1 節の評価用テンプレートとは変えてあります）。

英語の主語は 60 個であるため、過去指定文・時点無指定文がそれぞれ 300 文となります。`Qwen2.5-7B` の実験ではこの英語 300 文ずつを使用し、`Llama-3.1-8B` の実験ではさらに日本語の主語 45 個から作成した 225 文ずつを加えました（日本語テンプレートは `prompts/gate_templates_ja.json` を参照）。

### P6 head of government（行政府の長）

過去時点を指定した文

```text
In the year {Y}, who was the head of government of {S}? (Answer with the name only.)
Answer:

During {Y}, who was the head of government of {S}? (Answer with the name only.)
Answer:

Who was the head of government of {S} during {Y}? (Answer with the name only.)
Answer:

Who was the head of government of {S}? Please answer for the year {Y}. (Answer with the name only.)
Answer:

Who was the head of government of {S}? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
Who is the head of government of {S}? (Answer with the name only.)
Answer:

Who is the head of government of {S} right now? (Answer with the name only.)
Answer:

Who is the head of government of {S}? (Give 1 name only.)
Answer:

Who is the head of government of {S}? Please answer briefly. (Answer with the name only.)
Answer:

Who is the head of government of {S}? (One name; answer with the name only.)
Answer:
```

### P35 head of state（国家元首）

過去時点を指定した文

```text
In the year {Y}, who was the head of state of {S}? (Answer with the name only.)
Answer:

During {Y}, who was the head of state of {S}? (Answer with the name only.)
Answer:

Who was the head of state of {S} during {Y}? (Answer with the name only.)
Answer:

Who was the head of state of {S}? Please answer for the year {Y}. (Answer with the name only.)
Answer:

Who was the head of state of {S}? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
Who is the head of state of {S}? (Answer with the name only.)
Answer:

Who is the head of state of {S} right now? (Answer with the name only.)
Answer:

Who is the head of state of {S}? (Give 1 name only.)
Answer:

Who is the head of state of {S}? Please answer briefly. (Answer with the name only.)
Answer:

Who is the head of state of {S}? (One name; answer with the name only.)
Answer:
```

### P488 chairperson（議長）

過去時点を指定した文

```text
In the year {Y}, who was the chair of {S}? (Answer with the name only.)
Answer:

During {Y}, who was the chair of {S}? (Answer with the name only.)
Answer:

Who was the chair of {S} during {Y}? (Answer with the name only.)
Answer:

Who was the chair of {S}? Please answer for the year {Y}. (Answer with the name only.)
Answer:

Who was the chair of {S}? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
Who is the chair of {S}? (Answer with the name only.)
Answer:

Who is the chair of {S} right now? (Answer with the name only.)
Answer:

Who is the chair of {S}? (Give 1 name only.)
Answer:

Who is the chair of {S}? Please answer briefly. (Answer with the name only.)
Answer:

Who is the chair of {S}? (One name; answer with the name only.)
Answer:
```

### P286 head coach（監督）

過去時点を指定した文

```text
In the year {Y}, who was the head coach of {S}? (Answer with the name only.)
Answer:

During {Y}, who was the head coach of {S}? (Answer with the name only.)
Answer:

Who was the head coach of {S} during {Y}? (Answer with the name only.)
Answer:

Who was the head coach of {S}? Please answer for the year {Y}. (Answer with the name only.)
Answer:

Who was the head coach of {S}? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
Who is the head coach of {S}? (Answer with the name only.)
Answer:

Who is the head coach of {S} right now? (Answer with the name only.)
Answer:

Who is the head coach of {S}? (Give 1 name only.)
Answer:

Who is the head coach of {S}? Please answer briefly. (Answer with the name only.)
Answer:

Who is the head coach of {S}? (One name; answer with the name only.)
Answer:
```

### P169 chief executive officer（最高経営責任者）

過去時点を指定した文

```text
In the year {Y}, who was the chief executive officer of {S}? (Answer with the name only.)
Answer:

During {Y}, who was the chief executive officer of {S}? (Answer with the name only.)
Answer:

Who was the chief executive officer of {S} during {Y}? (Answer with the name only.)
Answer:

Who was the chief executive officer of {S}? Please answer for the year {Y}. (Answer with the name only.)
Answer:

Who was the chief executive officer of {S}? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
Who is the chief executive officer of {S}? (Answer with the name only.)
Answer:

Who is the chief executive officer of {S} right now? (Answer with the name only.)
Answer:

Who is the chief executive officer of {S}? (Give 1 name only.)
Answer:

Who is the chief executive officer of {S}? Please answer briefly. (Answer with the name only.)
Answer:

Who is the chief executive officer of {S}? (One name; answer with the name only.)
Answer:
```

### P127 owned by（所有者）

過去時点を指定した文

```text
In the year {Y}, who owned {S}? (Answer with the name only.)
Answer:

During {Y}, who owned {S}? (Answer with the name only.)
Answer:

Who owned {S} during {Y}? (Answer with the name only.)
Answer:

Who owned {S}? Please answer for the year {Y}. (Answer with the name only.)
Answer:

Who owned {S}? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
Who owns {S}? (Answer with the name only.)
Answer:

Who owns {S} right now? (Answer with the name only.)
Answer:

Who owns {S}? (Give 1 name only.)
Answer:

Who owns {S}? Please answer briefly. (Answer with the name only.)
Answer:

Who owns {S}? (One name; answer with the name only.)
Answer:
```

### P54 member of sports team（所属チーム）

過去時点を指定した文

```text
In the year {Y}, which team did {S} play for? (Answer with the name only.)
Answer:

During {Y}, which team did {S} play for? (Answer with the name only.)
Answer:

Which team did {S} play for during {Y}? (Answer with the name only.)
Answer:

Which team did {S} play for? Please answer for the year {Y}. (Answer with the name only.)
Answer:

Which team did {S} play for? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
Which team does {S} play for? (Answer with the name only.)
Answer:

Which team does {S} play for right now? (Answer with the name only.)
Answer:

Which team does {S} play for? (Give 1 name only.)
Answer:

Which team does {S} play for? Please answer briefly. (Answer with the name only.)
Answer:

Which team does {S} play for? (One name; answer with the name only.)
Answer:
```

### P39 position held（役職）

過去時点を指定した文

```text
In the year {Y}, what position did {S} hold? (Answer with the name only.)
Answer:

During {Y}, what position did {S} hold? (Answer with the name only.)
Answer:

What position did {S} hold during {Y}? (Answer with the name only.)
Answer:

What position did {S} hold? Please answer for the year {Y}. (Answer with the name only.)
Answer:

What position did {S} hold? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
What position does {S} hold? (Answer with the name only.)
Answer:

What position does {S} hold right now? (Answer with the name only.)
Answer:

What position does {S} hold? (Give 1 name only.)
Answer:

What position does {S} hold? Please answer briefly. (Answer with the name only.)
Answer:

What position does {S} hold? (One name; answer with the name only.)
Answer:
```

### P108 employer（雇用主）

過去時点を指定した文

```text
In the year {Y}, which organization did {S} work for? (Answer with the name only.)
Answer:

During {Y}, which organization did {S} work for? (Answer with the name only.)
Answer:

Which organization did {S} work for during {Y}? (Answer with the name only.)
Answer:

Which organization did {S} work for? Please answer for the year {Y}. (Answer with the name only.)
Answer:

Which organization did {S} work for? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
Which organization does {S} work for? (Answer with the name only.)
Answer:

Which organization does {S} work for right now? (Answer with the name only.)
Answer:

Which organization does {S} work for? (Give 1 name only.)
Answer:

Which organization does {S} work for? Please answer briefly. (Answer with the name only.)
Answer:

Which organization does {S} work for? (One name; answer with the name only.)
Answer:
```

### P102 member of political party（所属政党）

過去時点を指定した文

```text
In the year {Y}, which political party was {S} a member of? (Answer with the name only.)
Answer:

During {Y}, which political party was {S} a member of? (Answer with the name only.)
Answer:

Which political party was {S} a member of during {Y}? (Answer with the name only.)
Answer:

Which political party was {S} a member of? Please answer for the year {Y}. (Answer with the name only.)
Answer:

Which political party was {S} a member of? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
Which political party is {S} a member of? (Answer with the name only.)
Answer:

Which political party is {S} a member of right now? (Answer with the name only.)
Answer:

Which political party is {S} a member of? (Give 1 name only.)
Answer:

Which political party is {S} a member of? Please answer briefly. (Answer with the name only.)
Answer:

Which political party is {S} a member of? (One name; answer with the name only.)
Answer:
```

### P69 educated at（出身校）

過去時点を指定した文

```text
In the year {Y}, which school did {S} attend? (Answer with the name only.)
Answer:

During {Y}, which school did {S} attend? (Answer with the name only.)
Answer:

Which school did {S} attend during {Y}? (Answer with the name only.)
Answer:

Which school did {S} attend? Please answer for the year {Y}. (Answer with the name only.)
Answer:

Which school did {S} attend? (Situation in {Y}; answer with the name only.)
Answer:
```

時点無指定の文

```text
Which school does {S} attend? (Answer with the name only.)
Answer:

Which school does {S} attend right now? (Answer with the name only.)
Answer:

Which school does {S} attend? (Give 1 name only.)
Answer:

Which school does {S} attend? Please answer briefly. (Answer with the name only.)
Answer:

Which school does {S} attend? (One name; answer with the name only.)
Answer:
```

## 3. その他のプロンプト

### 3.1 同種の別主語を生成させるプロンプト

行列 $C_q$ の算出では、質問 $q$ の主語を同種の別主語へ置換した質問を用います。別主語は、以下の few-shot 例示の末尾に主語を連結し、適用前モデルに続きを貪欲法で生成させて取得しました（最大 64 トークン）。生成された最初の行をセミコロンで区切り、元の主語を含む名前および元の主語に含まれる名前を除外したうえで、先頭から最大 8 個を採用しています。例示は全事実で共通です。

```text
Toyota: Honda; Nissan; Mazda; Subaru; Suzuki; Mitsubishi; Hyundai; Kia
Lyon: Marseille; Toulouse; Nice; Nantes; Strasbourg; Montpellier; Bordeaux; Lille
Donau: Rhein; Elbe; Weichsel; Oder; Main; Mosel; Inn; Ems
富士山: 北岳; 奥穂高岳; 槍ヶ岳; 立山; 白山; 御嶽山; 浅間山; 八ヶ岳
{S}:
```

### 3.2 年を置換した 17 通りの質問

行列 $C$ の算出・時間ゲートの学習・保持集合に用いる 17 通りの年付き質問は、2023 年末を指定した質問 $q_{\mathrm{ref}}$（`method_qref`）中の `2023` を $2023 - k$（$k = 0, 1, \dots, 16$）に置換して作成しました。以下に P6 の例を示します。

```text
As of the end of 2023, who was the head of government of {S}? (Answer with the name only.)
Answer:

As of the end of 2022, who was the head of government of {S}? (Answer with the name only.)
Answer:

（中略。2021 から 2008 まで同じ形）

As of the end of 2007, who was the head of government of {S}? (Answer with the name only.)
Answer:
```

### 3.3 主語のみを入力とする生成

保持集合の一部は、質問文や指示を付与せず、主語ラベル `{S}` のみを入力として適用前モデルに続きを貪欲法で生成させて作成しました。

## 4. 追加分析用プロンプト

### 4.1 評価用テンプレートに含まれない 5 種類の時間表現

多様な時間表現に対する保持性能を検証するため、1 節のテンプレートにおける時間表現部分のみを置換したプロンプトを作成しました。なお、旧回答が成立していた期間（[DATA.md](DATA.md) の 3 節）の中央の日を「中央の日」と呼びます。

| 時間表現 | 作成方法 | P6 での例 |
|---|---|---|
| 年月日 | `reference_eval` の 2 文目の `December 31, 2023` を中央の日に置換する（期間が日単位で判明している事実のみ対象） | `Who was the head of government of {S} on December 29, 2014? …` |
| 年月 | `reference_eval` の 3 文目の `December 2023` を中央の日の年月に置換する（期間が年単位の事実では、中央の日の年が期間の開始年・終了年のいずれとも異なる場合のみ対象） | `In December 2014, who was the head of government of {S}? …` |
| 期間 | `retain_eval` の 1 文目の `As of {Y},` を `From {a} to {b},` に置換する（`{a}` は期間の開始年、`{b}` は旧回答が最後に成立していた年。両年が同一の事実は除外） | `From 2007 to 2022, who was the head of government of {S}? …` |
| 基準年からの相対表現 | `As of {Y},` を `{2023 − Y} years before 2023,` に置換する（差が 1 年の場合は `One year before 2023,` とする） | `9 years before 2023, who was the head of government of {S}? …` |
| 五輪の開催年 | `As of {Y},` を `In the year the {Summer または Winter} Olympics were held in {開催都市},` に置換する（`{Y}` が 2010, 2012, 2014, 2016, 2018, 2021, 2022 年の事実のみ対象） | `In the year the Winter Olympics were held in Sochi, who was the head of government of {S}? …` |

五輪の開催年と開催都市の対応は、2010 年 Vancouver（冬季）、2012 年 London（夏季）、2014 年 Sochi（冬季）、2016 年 Rio de Janeiro（夏季）、2018 年 Pyeongchang（冬季）、2021 年 Tokyo（夏季）、2022 年 Beijing（冬季）です。

### 4.2 質問の前に置いた無関係な文脈

質問の直前に文脈が置かれた場合の忘却・保持性能を検証するため、以下の 2 種類の文脈を時点無指定質問（`method_q`）および過去時点指定質問（`retain_eval` の 1 文目）の前に配置しました。

無関係な 1-shot 例：

```text
What is the chemical symbol for gold? (Answer with the symbol only.)
Answer: Au

```

指示文：

```text
Please answer the following question.

```

いずれの文脈も、末尾に空行を 1 行挟んでから質問を続けます。
