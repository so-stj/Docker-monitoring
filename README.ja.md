# Docker モニタリングスタック

Prometheus、Grafana、Alertmanager、Node Exporterを使用したシステム監視とアラート機能を提供する包括的なDockerベースのモニタリングソリューションです。

## 🚀 概要

このプロジェクトは以下の完全なモニタリングスタックを提供します：
- **Prometheus**: メトリクス収集のための時系列データベース
- **Grafana**: 可視化とダッシュボードプラットフォーム
- **Alertmanager**: アラートルーティングと通知管理
- **Node Exporter**: Prometheus用のシステムメトリクスエクスポーター

## 📋 前提条件

- Docker
- Docker Compose
- 最低2GBの利用可能RAM
- ポート3000、9090、9093、9100が利用可能

## 🛠️ インストールとセットアップ

1. **リポジトリのクローン**
   ```bash
   git clone <repository-url>
   cd Docker-monitoring
   ```

2. **Alertmanagerの設定（オプション）**
   `alertmanager/config.yaml`を編集してSlack webhook URLを追加：
   ```yaml
   global:
     slack_api_url: "your-slack-webhook-url"
   ```

3. **モニタリングスタックの起動**
   ```bash
   docker-compose up -d
   ```

## 🌐 アクセスポイント

スタックが起動すると、以下のサービスにアクセスできます：

| サービス | URL | デフォルト認証情報 |
|---------|-----|-------------------|
| **Grafana** | http://localhost:3000 | admin/admin |
| **Prometheus** | http://localhost:9090 | なし |
| **Alertmanager** | http://localhost:9093 | なし |
| **Node Exporter** | http://localhost:9100 | なし |

## 📊 設定

### Prometheus設定
- **スクレイプ間隔**: 15秒
- **評価間隔**: 15秒
- **ターゲット**: Prometheus自身とNode Exporter

### アラートルール
システムには以下の基本的なアラートルールが含まれています：
- インスタンスダウン検出（5分間の閾値）
- 重大度の分類

### Alertmanager
- Slack通知用に設定
- アラート配信のルート設定
- チャンネル: #alerts

## 🔧 カスタマイズ

### 新しいターゲットの追加
追加のサービスを監視するには、`prometheus/prometheus.yaml`を編集：
```yaml
scrape_configs:
  - job_name: 'your-service'
    static_configs:
      - targets:
        - 'your-service:port'
```

### カスタムアラートの作成
`prometheus/alert.rules.yaml`に新しいアラートルールを追加：
```yaml
- alert: your_alert_name
  expr: your_promql_expression
  for: 5m
  labels:
    severity: warning
  annotations:
    summary: "アラート概要"
    description: "詳細な説明"
```

## 📈 Grafanaの開始方法

1. http://localhost:3000 でGrafanaにアクセス
2. admin/adminでログイン
3. Prometheusをデータソースとして追加：
   - URL: `http://prometheus:9090`
   - アクセス: サーバー（デフォルト）
4. ダッシュボードをインポートまたは独自に作成

## 🚨 アラート機能

システムは以下の場合にSlackにアラートを送信するように設定されています：
- インスタンスが5分以上ダウンした場合
- アラートルールで定義されたその他の条件

Slack通知を設定するには：
1. Slackアプリを作成してwebhook URLを取得
2. `alertmanager/config.yaml`を更新
3. alertmanagerサービスを再起動

## 🛑 スタックの停止

```bash
docker-compose down
```

すべてのデータを削除する場合：
```bash
docker-compose down -v
```

## 📁 プロジェクト構造

```
Docker-monitoring/
├── docker-compose.yml          # メインオーケストレーションファイル
├── prometheus/
│   ├── prometheus.yaml         # Prometheus設定
│   └── alert.rules.yaml        # アラートルール
├── alertmanager/
│   └── config.yaml             # Alertmanager設定
└── README.md                   # このファイル
```

## 🔍 トラブルシューティング

### 一般的な問題

1. **ポート競合**: ポート3000、9090、9093、9100が利用可能であることを確認
2. **権限の問題**: 適切なDocker権限で実行
3. **設定エラー**: 設定ファイルのYAML構文を確認

### ログ
特定のサービスのログを表示：
```bash
docker-compose logs prometheus
docker-compose logs grafana
docker-compose logs alertmanager
```
