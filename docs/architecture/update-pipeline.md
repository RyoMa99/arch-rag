# インデックス更新パイプライン（Update Pipeline）

## パイプライン図

```mermaid
graph TD
    subgraph UpdatePipeline["インデックス更新パイプライン"]
        UpdateTrigger[更新トリガー] --> UpdateStrategy{更新戦略}

        UpdateStrategy -->|immediate| ImmediateUpdate[即時更新]
        UpdateStrategy -->|batch| BatchQueue[バッチキュー]

        BatchQueue --> BatchProcessor[バッチ処理]

        ImmediateUpdate --> GhostDataChecker[ゴーストデータ検査]
        BatchProcessor --> GhostDataChecker

        GhostDataChecker --> DeleteOldVectors[古いベクトル削除]
        DeleteOldVectors --> InsertNewVectors[新規ベクトル挿入]

        InsertNewVectors --> IndexValidation[インデックス検証]
        IndexValidation --> HotColdMigration{ホット/コールド移行}

        HotColdMigration -->|promote| PromoteToHot[ホットへ昇格]
        HotColdMigration -->|demote| DemoteToCold[コールドへ降格]
        HotColdMigration -->|keep| END3((完了))

        PromoteToHot --> END3
        DemoteToCold --> END3
    end
```

## 更新時の課題

- チャンク境界のズレ・文脈分断
- ゴーストデータ（古いベクトル）によるハルシネーション
- ベクトルストアのインデックスの断片化
- 定期的なインデックス再構築が必要

## ノード詳細

### 更新トリガー

- リソース更新の検知

### 更新戦略

| 戦略 | 説明 |
|------|------|
| `immediate` | リアルタイムで即時更新 |
| `batch` | 更新をキューに蓄積し、まとめて処理 |

### ホット/コールド移行

| アクション | 説明 |
|-----------|------|
| `promote` | コールドからホットへ昇格 |
| `demote` | ホットからコールドへ降格 |
| `keep` | 現状維持 |
