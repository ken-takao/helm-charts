# Helm Charts

このリポジトリには、Kubernetesで使用するためのHelm Chartsが含まれています。Teslaの[fleet-telemetry](https://github.com/teslamotors/helm-charts/tree/main/charts/fleet-telemetry)チャートをフォークして、AWSリージョンを指定できるように修正しました。

## 使用方法

### リポジトリの追加

```bash
helm repo add ken-takao https://ken-takao.github.io/helm-charts/
helm repo update
```

### 利用可能なチャートの検索

```bash
helm search repo ken-takao
```

### fleet-telemetryチャートのインストール

```bash
helm install fleet-telemetry ken-takao/fleet-telemetry \
  --set config.aws.region=ap-northeast-1 \
  --set serviceAccount.create=true \
  --set serviceAccount.annotations."eks\.amazonaws\.com/role-arn"=arn:aws:iam::123456789012:role/KinesisRole
```

## 修正内容

### fleet-telemetry

1. AWSリージョンを指定できるようにしました
   - `values.yaml`に`config.aws.region`の設定を追加
   - Deploymentテンプレートで環境変数として`AWS_REGION`を設定

2. ServiceAccountのサポートを追加しました
   - IAMロールをServiceAccountにアタッチするための設定を追加

## ローカルでのテスト方法

```bash
# リポジトリのクローン
git clone https://github.com/ken-takao/helm-charts.git
cd helm-charts

# チャートのパッケージ化
helm package charts/fleet-telemetry -d .

# ローカルでのインストールテスト
helm install --dry-run --debug fleet-telemetry ./fleet-telemetry-*.tgz
```
