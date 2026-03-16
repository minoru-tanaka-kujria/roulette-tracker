# 🎰 Roulette Tracker

ルーレットの出目トレンドをリアルタイム追跡するPWAアプリ。
スマホのホーム画面に追加すればネイティブアプリのように使えます。

## 機能
- 0 / 00 / 1〜36 の全出目入力（アメリカンルーレット）
- 赤黒 / 奇偶 / ハイロー / ダズン / カラム の偏り分析
- 各カテゴリの回数・確率をリアルタイム表示
- バカラ風罫線＆連続パターン（タップで詳細表示）
- 偏りに基づくレコメンド（娯楽目的）
- 履歴はlocalStorageに自動保存
- オフライン対応（Service Worker）

## デプロイ方法

### 方法1: Vercel（おすすめ・最速）

1. GitHubにリポジトリを作成してこのフォルダをpush
```bash
git init
git add .
git commit -m "initial"
git remote add origin https://github.com/YOUR_USER/roulette-tracker.git
git push -u origin main
```

2. [vercel.com](https://vercel.com) にGitHubでログイン
3. 「New Project」→ リポジトリを選択 → Deploy
4. 完了！URLが発行されるので友達にシェア

### 方法2: Netlify

1. [app.netlify.com](https://app.netlify.com) にログイン
2. 「Sites」→ `public` フォルダをドラッグ＆ドロップ
3. 完了！

### 方法3: Cloudflare Pages

1. [pages.cloudflare.com](https://pages.cloudflare.com) にログイン
2. GitHubリポジトリを接続
3. Build output directory: `public`
4. Deploy

## スマホでアプリ化する方法

デプロイしたURLをスマホのブラウザで開いて：

**iPhone (Safari)**
→ 共有ボタン → 「ホーム画面に追加」

**Android (Chrome)**
→ メニュー(⋮) → 「ホーム画面に追加」 or 「アプリをインストール」

## ファイル構成
```
roulette-pwa/
├── vercel.json          # Vercel設定
├── README.md
└── public/
    ├── index.html       # メインアプリ
    ├── manifest.json    # PWAマニフェスト
    ├── sw.js            # Service Worker（オフライン対応）
    ├── icon-192.svg     # アプリアイコン
    └── icon-512.svg     # アプリアイコン（大）
```

## カスタムドメイン
Vercelの場合、Settings → Domains から好きなドメインを設定可能。
例: `roulette.yourdomain.com`
