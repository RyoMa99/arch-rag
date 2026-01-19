# RAGシステム アーキテクチャ設計

LangGraphで実装する運用向けRAGシステムのアーキテクチャ設計ドキュメント。

## 概要

本システムは3つの主要パイプラインで構成される。

## パイプライン一覧

| パイプライン | ドキュメント | 説明 |
|-------------|-------------|------|
| 検索パイプライン | [query-pipeline.md](./query-pipeline.md) | ユーザークエリを処理し、関連ドキュメントを検索して回答を生成 |
| インデックス作成パイプライン | [indexing-pipeline.md](./indexing-pipeline.md) | ドキュメントをチャンク化、ベクトル化してインデックスに登録 |
| 更新パイプライン | [update-pipeline.md](./update-pipeline.md) | 既存インデックスの更新、ゴーストデータ削除、ホット/コールド移行 |

## 運用ドキュメント

| ドキュメント | 説明 |
|-------------|------|
| [operations.md](./operations.md) | 監視メトリクス、アラート条件、ダッシュボード設計 |

## 技術スタック（参考）

| カテゴリ | 推奨技術 |
|---------|---------|
| フレームワーク | LangGraph |
| 埋め込みモデル | ruri-v3-310m（日本語特化、JMTEBでSOTA） |
| ベクトルDB | Qdrant / Weaviate |
| 全文検索 | Elasticsearch (BM25) |
| Reranker | TBD |
| 評価 | RAGAS + Langfuse |
