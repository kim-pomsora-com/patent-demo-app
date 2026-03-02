# Execution Plan Integrity MVP

## 技術的特異点（審査官向け解説）

本デモンストレーションは、特許請求項1における「推論対象空間の動的再構成」の物理的実装を証明するものである。

### 従来技術との決定的な違い
従来のAI推論やグラフ処理（Pruning, Masking）の多くは、グラフ構造を維持したまま、実行時に「フラグ」や「条件分岐（if文）」によって演算をスキップする。
この場合、制御フロー（Instruction Fetching / Decoding）レベルでは依然として「全ノード」にアクセスするコストが発生し、構造的な分離は行われない。

### 本発明の実装
本発明のデモでは、アンカー $L$ に基づき、**実行時リスト（Execution Plan List）そのものを動的に再構成**する。
1. **構造的排除**: 実行ループ `for node in execution_plan_list:` が開始される時点で、不要なノードはメモリ上のアクセス対象リストから物理的に抹消されている。
2. **決定論的不在**: デモ内の `CallTracer` は、実行計画に含まれないノードへのアクセスが「0回」であることを証明する。これは条件分岐による「0」ではなく、イテレータの構造そのものに起因する。

## セットアップ手順

1. Python 3.9+ の環境を用意。
2. 依存関係のインストール:
   ```bash
   pip install -r requirements.txt
   ```
3. `secrets.toml` の設定:
   `.streamlit/secrets.toml` を作成し、以下を記述。
   ```toml
   APP_PASSWORD = "admin"
   ```
4. 起動:
   ```bash
   streamlit run app.py
   ```
