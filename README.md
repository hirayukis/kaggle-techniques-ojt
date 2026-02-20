# Kaggle Techniques OJT

このリポジトリは、**「手法を足す前に split 戦略を正しく設計しないと、どんな工夫も悪化しうる」** ことを体験的に学ぶための教材です。

ポイントは次の 2 つです。

- `00_prepare_data.ipynb` は**教材用データ生成ノート**（唯一のデータ作成ステップ）
- `01` 〜 `07` は、同じデータを使って**分割戦略・検証戦略の違い**を比較する実験ノート

## 学習ゴール

- Train/Valid/Test の役割を混同しない評価設計を身につける
- CV（特に StratifiedKFold）の必要性を、結果差で理解する
- 「手法追加（Optuna/特徴量エンジニアリング）」より先に、評価設計の健全性を担保する感覚を得る

## Notebookロードマップ（00〜07）

### 00: データ生成（唯一の前処理・生成ノート）

- `00_prepare_data.ipynb`
	- 分類データを生成し、`data/processed/classification_train.csv` と `classification_test.csv` を保存
	- 設定は `config/dataset_config.json` を使用
	- ここで作ったデータを `01` 〜 `07` が共通利用

### 01: 最小ベースライン

- `01_lightgbm_starter.ipynb`
	- LightGBM の基本学習（シンプルな分割）
	- Train / Valid / Test のメトリクス確認

### 02: Stratified split 比較

- `02_lightgbm_stratified_compare.ipynb`
	- 通常 split と stratified split の比較
	- 分割方法だけで指標がどう変わるかを確認

### 03: KFold vs 単発 split

- `03_kfold_vs_simple_inference.ipynb`
	- KFold 推論と単発 split 推論の比較
	- 推論安定性・平均化効果を確認

### 04: StratifiedKFold vs 単発 split

- `04_stratified_kfold_vs_simple_inference.ipynb`
	- StratifiedKFold を使った場合の比較
	- クラスバランスを意識した CV の挙動確認

### 05: Pattern比較 + Adversarial Validation

- `05_stratified_kfold_pattern1_pattern2_adversarial.ipynb`
	- Pattern1 / Pattern2 を StratifiedKFold で比較
	- Adversarial Validation により Train vs Test の分布差が強い特徴を除外
	- fold ごとの dropped features を出力

### 06: Optuna あり / なし比較

- `06_optuna_vs_no_optuna.ipynb`
	- 同一 split 設計で、Optuna あり・なしの差を比較
	- 「探索を入れても split が悪いと改善しない」ことを検証

### 07: 特徴量エンジニアリング比較

- `07_feature_engineering_target_encoding_vs_baseline.ipynb`
	- Baseline vs Advanced（特徴量エンジニアリング + Frequency Encoding）
	- Validation 過学習につながりやすい追加特徴を除外して比較

## この教材で一番伝えたいこと

**split 戦略が不適切な状態では、手法を高度化しても悪化することがある**、という点です。

具体的には、次の順序を守るのが重要です。

1. 先に評価設計（Train/Valid/Test の分離、CV 方針）を固定する
2. その上で特徴量やハイパーパラメータ探索を追加する
3. 改善判定は Test を汚染せずに行う

## セットアップ

### Docker（推奨）

```bash
docker-compose up --build
# http://localhost:8888
```

### ローカル

```bash
python -m venv .venv
source .venv/bin/activate  # macOS/Linux
# .venv\Scripts\activate  # Windows

pip install -r requirements.txt
jupyter notebook
```

## 推奨実行順

1. `00_prepare_data.ipynb`
2. `01` → `02` → `03` → `04`
3. `05` → `06` → `07`

## 注意事項

- `00` 以外のノートでデータ生成処理は行わない
- 実験比較時は、同じ split 条件を揃えて差分を見る
- Test は最終確認専用（チューニングに使わない）
