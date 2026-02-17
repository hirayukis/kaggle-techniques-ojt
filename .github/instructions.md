# Notebook作成ガイドライン

## データ分割の必須ルール

**すべてのNotebookにおいて、以下のルールを厳守すること：**

### 1. データは必ずTrain/Valid/Testの3つに分割する

- **Train**: モデルの学習に使用
- **Validation**: ハイパーパラメータチューニングや早期停止の判断に使用
- **Test**: 最終的なモデル性能評価にのみ使用

### 2. Testセットは学習に一切使用しない

- Testセットはモデルの学習、検証、パラメータ調整には使用しない
- 最終的な性能評価のためだけに使用する
- データリークを防ぐため、最後まで触らない

### 3. 推奨される分割比率

- Train: 60%
- Validation: 20%
- Test: 20%

または

- Train: 70%
- Validation: 15%
- Test: 15%

### 実装例

```python
from sklearn.model_selection import train_test_split

# まずTestセットを分離（学習には一切使用しない）
X_train_full, X_test, y_train_full, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# TrainセットをTrainとValidに分割
X_train, X_valid, y_train, y_valid = train_test_split(
    X_train_full, y_train_full, test_size=0.25, random_state=42
)
# 結果: Train 60%, Valid 20%, Test 20%
```

---

**このルールに従わないNotebookは受け入れられません。**
