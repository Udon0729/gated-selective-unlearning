# Gated Selective Unlearning (GSU) 補足資料

本リポジトリは、以下の論文に関する補足資料（実験で使用したプロンプト全文および評価データの構築手順）を公開するものです。

## 出典

- **論文題目**：学習済み言語モデルが内部にもつ新旧知識の混同の解消
  （*Post-Hoc Resolution of Conflicts between Outdated and Updated Knowledge in Language Models*）
- **著者**：宗岡 康太，狩野 芳伸（静岡大学 情報学部）
- **発表**：人工知能学会 第3回計算創律科学研究会（SIG-CSS）（人工知能学会 合同研究会2026 / SIGAIs 2026、2026年12月3日）

## 公開資料一覧

| ファイル | 内容 |
|---|---|
| [PROMPTS.md](PROMPTS.md) | 対象事実の選定・評価・時間ゲート学習・追加分析に使用したプロンプトテンプレート全文 |
| [DATA.md](DATA.md) | 元データからの事実抽出、旧回答・新回答の定義、データ分割、正誤判定規則、評価データ構築手順 |
| [prompts/templates_en.json](prompts/templates_en.json) | 11 関係に対する英語評価プロンプトテンプレート（機械可読形式） |
| [prompts/gate_templates_en.json](prompts/gate_templates_en.json) | 時間ゲートの学習に用いた固定文テンプレート（英語） |
| [prompts/gate_templates_ja.json](prompts/gate_templates_ja.json) | 時間ゲートの学習に用いた固定文テンプレート（日本語、Llama-3.1-8B の実験で使用） |

## 実験対象の概要

- **対象モデル**：`meta-llama/Llama-3.1-8B` および `Qwen/Qwen2.5-7B`（いずれも事前学習済みベースモデルであり、指示チューニング・対話チューニングは施していない）
- **元データ**：PTC（Hossain et al., arXiv:2606.20959）および TempLAMA（Dhingra et al., TACL 2022）
- **評価言語**：英語

※ PTC および TempLAMA のデータセット本体、ならびに実験コードは本リポジトリには含まれません。
