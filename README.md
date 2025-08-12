# local_LLM

ローカル実行のLLMを比較・評価・実験するためのテンプレートプロジェクトです。  
**目的**: GPU購入前のプレ調査〜小規模PoCの再現性確保。

## 特長
- Transformersベースの**軽量評価ハーネス**（4bit/8bit対応）
- **標準化プロンプト**と**Run Log**で横並び比較
- GradioによるシンプルなチャットUI
- pre-commit / GitHub Actionsで最低限の品質担保

## クイックスタート
```bash
# 1) 仮想環境の作成（例：uv か venv か condaを使用）
python -m venv .venv && source .venv/bin/activate  # Windowsは .venv\Scripts\activate

# 2) 依存関係
pip install -U pip wheel
pip install -r requirements.txt

# 3) Hugging Face 認証（必要なら）
# export HF_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
# または .env に書く（.env.example を参照）

# 4) ハード情報の把握
python scripts/collect_hw_info.py

# 5) ベンチ（Swallowを例）
python scripts/bench.py --config configs/models/swallow-8b.yaml --prompts prompts/prompt_set.csv --out runs/swallow

# 6) Gradioでチャット
python scripts/serve_gradio.py --config configs/models/elyza-8b.yaml
```

## ディレクトリ
- `configs/` : モデルや推論設定（YAML）
- `prompts/` : 評価用プロンプト（CSV/JSONL推奨）
- `scripts/` : CLIユーティリティ（ベンチ、UI、ハード情報）
- `src/local_llm/` : ライブラリ本体（ローダ、評価関数など）
- `tests/` : 単体テスト（CI用）
- `.github/workflows/` : GitHub Actions

## ライセンスとモデルの取り扱い
- このテンプレは **MIT License**。
- 各モデルの**ライセンス（商用可否・禁止行為）**はモデルカードに従ってください。
- **モデル重みはコミットしない**（.gitignore済）。必要に応じて`huggingface-cli`やOllamaで取得。

## トラブルシュート
- 4bit読み込みがエラーのとき：CUDA/torch/bitsandbytesの組合せを再確認
- FA2が使えないとき：`attn_implementation: sdpa` に変更
