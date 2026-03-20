# ACT Connect IoT API 仕様

ACT Connect カーテン制御システムの MQTT メッセージ仕様を AsyncAPI 3.0 形式で定義しています。

## ドキュメント閲覧

**GitHub Pages:** https://act-node.github.io/act-api-open/

## ファイル構成

```
docs/
├── asyncapi.yaml          # AsyncAPI メイン仕様
├── index.html             # ドキュメント表示ページ（GitHub Pages用）
└── schemas/
    ├── command.json       # コマンドメッセージスキーマ
    ├── response.json      # レスポンスメッセージスキーマ
    ├── event_limiter.json         # リミッターイベントスキーマ
    ├── event_emergency_stop.json  # 緊急停止イベントスキーマ
    ├── event_motor_state.json     # モーター状態変化イベントスキーマ
    ├── event_mode_change.json     # モード変更イベントスキーマ
    ├── telemetry.json     # テレメトリスキーマ
    └── health.json        # ヘルスチェックスキーマ

.github/
└── workflows/
    └── deploy.yml         # GitHub Pages 自動デプロイ
```

## トピック一覧

| 方向 | トピック | 用途 | QoS |
|------|---------|------|-----|
| Down | `{device_id}/curtain/command`  | モーター制御指示 | 1 |
| Up   | `{device_id}/curtain/response` | コマンド実行結果 | 1 |
| Up   | `{device_id}/curtain/event`    | 状態変化イベント | 1 |
| Up   | `{device_id}/sensor/telemetry` | 定期センサーデータ（30秒） | 0 |
| Up   | `{device_id}/curtain/health`   | ヘルスチェック（300秒） | 0 |

## ローカルでの確認方法

### AsyncAPI Studio（ブラウザ）

1. [https://studio.asyncapi.com](https://studio.asyncapi.com) を開く
2. `docs/asyncapi.yaml` の内容を貼り付ける

### AsyncAPI CLI

```bash
npm install -g @asyncapi/cli
asyncapi validate docs/asyncapi.yaml
asyncapi generate fromTemplate docs/asyncapi.yaml @asyncapi/html-template -o ./output
```

## GitHub Pages 公開手順

1. このリポジトリを GitHub に push する
2. Settings → Pages → Source を **GitHub Actions** に設定
3. `main` ブランチに push すると自動でデプロイされる

## デバイス構成

| device_id | 種別 | 機能 |
|-----------|------|------|
| `x320-curtain-01` | カーテン開閉デバイス | モーター M1/M2・温度センサー内蔵 |
| `x320-co2-01`     | CO2センサーデバイス | CO2・温度・湿度 |
