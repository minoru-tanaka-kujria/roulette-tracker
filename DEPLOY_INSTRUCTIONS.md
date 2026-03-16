# Roulette Tracker — Vercelデプロイ要件定義書（Claude Code指示書）

## ゴール

`roulette-pwa/` フォルダ内の静的PWAアプリをGitHubリポジトリにpushし、Vercelに本番デプロイする。
デプロイ完了後、動作確認を行い、本番URLを報告すること。

---

## プロジェクト概要

- **アプリ名**: Roulette Tracker
- **用途**: アメリカンルーレット（0, 00, 1〜36）の出目をリアルタイム追跡し、偏り分析＆レコメンドを表示するPWA
- **技術構成**: フレームワークなし。vanilla HTML/CSS/JS の1ファイル完結（`public/index.html`）
- **デプロイ先**: Vercel（静的サイト）

---

## ファイル構成

```
roulette-pwa/
├── vercel.json          # Vercel設定（outputDirectory: "public"）
├── README.md            # プロジェクト説明
└── public/
    ├── index.html       # アプリ本体（全ロジック内包）
    ├── manifest.json    # PWAマニフェスト（standalone, アイコン定義）
    ├── sw.js            # Service Worker（オフラインキャッシュ）
    ├── icon-192.svg     # アプリアイコン 192x192
    └── icon-512.svg     # アプリアイコン 512x512
```

---

## 実行手順

以下をターミナルで順番に実行すること。Claude Code内ではなく、**ターミナルで実行するコマンド**である。

### 1. 環境確認

```bash
gh --version
vercel --version
```

- `gh` が入っていなければ: `brew install gh` → `gh auth login`
- `vercel` が入っていなければ: `npm install -g vercel` → `vercel login`

### 2. GitHubリポジトリ作成 & push

```bash
cd roulette-pwa
git init
git add .
git commit -m "feat: roulette tracker PWA v1.0"
gh repo create roulette-tracker --public --source=. --push
```

- リポジトリ名は `roulette-tracker` とする
- publicリポジトリでOK

### 3. Vercelデプロイ

```bash
vercel --yes --prod
```

- 初回のみプロジェクト設定を聞かれる場合がある。以下で回答:
  - Scope → 自分のアカウント
  - Link to existing project → N
  - Project name → roulette-tracker
  - Directory → ./
  - Override settings → N

### 4. 動作確認

デプロイ完了後に表示されるURLを開き、以下を**すべて**確認:

- [ ] ページが正常に表示される（ダークテーマ、ゴールドのヘッダー）
- [ ] 入力欄に `7` と入力してEnterまたは「入力」ボタン → 履歴チップに赤い7が追加される
- [ ] 入力欄に `00` と入力 → 緑の00チップが追加される
- [ ] 入力欄に `0` と入力 → 緑の0チップが追加される（00とは別カウント）
- [ ] 赤/黒/0/00 の4カテゴリそれぞれに回数と%が表示される
- [ ] 奇偶・ハイロー・ダズン・カラムに回数と%が表示される
- [ ] 5回以上入力すると、偏りがあればレコメンドが表示される
- [ ] 「↩ 戻す」ボタンで直前の入力が取り消せる
- [ ] 「リセット」ボタンで全データクリア（確認ダイアログあり）
- [ ] ブラウザを閉じて再度URLを開いても履歴が残っている（localStorage）

### 5. 結果報告

以下を報告すること:
- デプロイURL（例: `https://roulette-tracker-xxxxx.vercel.app`）
- 動作確認結果（上記チェックリスト全項目のpass/fail）

---

## 技術仕様（参考情報）

- 00 は内部的に `-1` として history 配列に格納
- 履歴は localStorage キー `rt5` に JSON 配列で保存
- Service Worker: ネットワークファースト + キャッシュフォールバック戦略
- フォント: Google Fonts CDN（DM Mono, Noto Sans JP）
- PWA: standalone 表示、ホーム画面追加でフルスクリーン動作

## 更新デプロイ手順（将来の変更時）

```bash
cd roulette-pwa
git add .
git commit -m "fix: 修正内容をここに書く"
git push
```

GitHub連携済みの場合、pushだけでVercelが自動再デプロイする。
手動の場合は `vercel --prod` を再実行。

---

## 注意事項

- `vercel.json` の `outputDirectory` が `"public"` になっていること。これがないと404になる。
- Vercelは自動でHTTPS。PWAのService Worker登録にHTTPSが必要なため、Vercel以外にデプロイする場合はHTTPS設定を忘れないこと。
- カスタムドメインを設定する場合: `vercel domains add ドメイン名`
