# スケマチ (Sukemachi) - スケジュール日程調整 Web アプリ

ログイン不要・URL共有で誰でも爆速で日程調整・出欠確認ができるWebサービスです。
Firebase Hosting & Firestore をバックエンドとして利用しています。

- **公開URL**: [https://sukemachi.web.app](https://sukemachi.web.app)
- **Firebase プロジェクト**: `yukiyanart-feedback-hub` (Hosting ターゲット: `sukemachi`)

---

## 💡 特徴・主な機能

1. **ログイン不要・URL共有完結**
   - イベント作成者は日付と時間帯を選ぶだけで即座に専用URLを発行。
   - 参加者は届いたURLを開くだけで、インストールやアカウント登録なしに出欠（◯・△・×）を入力可能。
2. **直感的な出欠入力フォーム**
   - 各候補日ごとのワンタップボタン式（◯・△・×）。
   - 「すべて◯にする」一括ボタンや、30分遅刻などの時間微調整メモ機能。
3. **出欠マトリクス表 & 自動集計**
   - 参加者の回答をリアルタイムに集計。最多出席日時の自動判定。
4. **管理者向け機能（4桁PIN保護）**
   - 作成時に設定した任意の4桁PINで管理画面にアクセス。
   - 候補日時の追加・修正（既存の回答データを保持したまま安全にマージ）。
   - 決定日時の確定（単日確定 / 複数日確定）と、メール・Slack・LINE用案内文面の自動生成。
5. **クラウドリアルタイム同期 & PWA・通知対応**
   - Firestoreによるリアルタイム同期（複数端末・異なるブラウザ間で即時反映）。
   - スマホの「ホーム画面に追加」対応（PWAマニフェスト、アプリアイコン完備）。
   - Web Notificationによる新規回答プッシュ通知。

---

## 🛠 技術スタック

- **フロントエンド**: HTML5, Tailwind CSS, Lucide Icons, Vanilla JavaScript
- **バックエンド / インフラ**: 
  - Firebase Hosting
  - Cloud Firestore（リアルタイムデータ同期）
- **PWA**: Service Worker (`sw.js`), Web App Manifest (`manifest.json`)

---

## 📁 ディレクトリ構成

```text
chosei-proto/
├── index.html           # メインアプリケーション（SPA）
├── manifest.json        # PWAマニフェスト設定
├── sw.js                # Service Worker（PWA / プッシュ通知）
├── firebase.json        # Firebase Hosting / Firestore 設定
├── firestore.rules      # Firestore セキュリティルール
├── .firebaserc          # Firebase プロジェクト設定
├── favicon.ico          # マルチ解像度ファビコン
├── apple-touch-icon.png # iOS ホーム画面アプリアイコン (180x180)
├── icon-16.png / 32.png # ファビコン各種サイズ
└── icon-192.png / 512.png# PWA / Android アイコン
```

---

## 🚀 デプロイ手順

Firebase CLI を使用してデプロイします。

```powershell
# Hosting のみデプロイ
firebase deploy --only hosting

# Firestore ルールのみデプロイ
firebase deploy --only firestore:rules
```

---

## 🔒 セキュリティと認証仕様について

- **管理PIN**:
  - イベント作成時に4桁の数字PINを設定（万能初期値・フォールバックは撤廃済み）。
  - 各イベント固有のPIN入力でのみ管理画面へアクセス可能。
- **データ保持ポリシー**:
  - ローカルストレージ（オフラインキャッシュ / 自分の回答記憶）と Firestore（クラウド同期）のハイブリッド構成。
