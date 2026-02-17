# Kaggle Techniques OJT

機械学習の学習用リポジトリ

## セットアップ

### Docker環境（推奨）

```bash
# イメージをビルドして起動
docker-compose up --build

# ブラウザで以下にアクセス
# http://localhost:8888
```

### ローカル環境

```bash
# venv作成
python -m venv .venv
source .venv/bin/activate  # macOS/Linux
# .venv\Scripts\activate  # Windows

# ライブラリインストール
pip install -r requirements.txt

# Jupyter起動
jupyter notebook
```

## データ分割ルール

すべてのNotebookは以下のルールに従うこと：

- データは必ず **Train/Valid/Test** の3つに分割
- **Testセット**は学習に一切使用せず、最終評価のみ
- 詳細は [.github/instructions.md](.github/instructions.md) を参照
