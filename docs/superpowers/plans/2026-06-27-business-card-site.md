# 名刺代わりサイト 実装計画

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** AIイラスト・ニックネーム・一言メッセージだけの1ページ完結型名刺サイトをHTML+CSSで作成する

**Architecture:** `index.html` に構造、`style.css` にスタイルを分離した2ファイル構成。外部ライブラリ不使用。画像は `images/` フォルダに配置する。

**Tech Stack:** HTML5, CSS3（Flexbox, グラデーション, レスポンシブ）

## Global Constraints

- 外部CDN・JavaScriptライブラリ不使用
- スマホ・PC両対応（レスポンシブ）
- 個人情報（実名・会社名・連絡先）は掲載しない
- カラーテーマ：明るいグラデーション（黄〜オレンジ系）
- イラストは円形トリミング表示

---

## ファイル構成

```
DEMO/
├── index.html          # メインHTML（カード構造）
├── style.css           # スタイル（背景・カード・イラスト・フォント）
└── images/
    └── avatar.png      # AIイラスト画像（ユーザーが差し替え）
```

---

### Task 1: フォルダ構成とプレースホルダー画像を準備する

**Files:**
- Create: `images/` フォルダ
- Create: `images/avatar.png`（プレースホルダー — 後でユーザーがAIイラストに差し替え）

**Interfaces:**
- Produces: `images/avatar.png`（Task 2 の `<img>` が参照する）

- [ ] **Step 1: `images` フォルダを作成し、プレースホルダー画像を配置する**

  `images/` フォルダを作成する。プレースホルダーとして、SVGデータURIではなく下記の1×1px PNG（base64）を `avatar.png` として保存する（後でAI生成画像に上書き差し替え）。

  実際には Task 2 で `<img>` の `src` に `images/avatar.png` を指定するだけでよい。ファイルが存在しなくても HTML は動作するため、このステップは「フォルダだけ作成」でよい。

  ```
  mkdir images
  ```

- [ ] **Step 2: Commit**

  ```bash
  git add images/.gitkeep
  git commit -m "chore: add images folder"
  ```

  ※ `images/` が空の場合、`.gitkeep` という空ファイルを置いてコミットする。

---

### Task 2: HTML構造を作成する

**Files:**
- Create: `index.html`

**Interfaces:**
- Consumes: `images/avatar.png`（イラスト画像）、`style.css`（スタイル）
- Produces: ブラウザで開けるHTMLページ

- [ ] **Step 1: `index.html` を作成する**

  以下の内容をそのまま `index.html` として保存する。ニックネームと一言メッセージは仮テキストを入れる（後でユーザーが書き換える）。

  ```html
  <!DOCTYPE html>
  <html lang="ja">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Page</title>
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <div class="card">
      <img class="avatar" src="images/avatar.png" alt="アバター">
      <h1 class="nickname">ニックネーム</h1>
      <p class="message">大学×ICTが好きです</p>
    </div>
  </body>
  </html>
  ```

- [ ] **Step 2: ブラウザで開いて構造を確認する**

  `index.html` をブラウザで開く。スタイルはまだないが、イラスト・ニックネーム・メッセージが縦に並んで表示されることを確認する。

- [ ] **Step 3: Commit**

  ```bash
  git add index.html
  git commit -m "feat: add HTML structure for business card page"
  ```

---

### Task 3: CSSスタイルを作成する

**Files:**
- Create: `style.css`

**Interfaces:**
- Consumes: `index.html` の `.card`、`.avatar`、`.nickname`、`.message` クラス
- Produces: グラデーション背景、中央配置カード、円形イラスト、レスポンシブ対応

- [ ] **Step 1: `style.css` を作成する**

  以下の内容をそのまま `style.css` として保存する。

  ```css
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    background: linear-gradient(135deg, #FFE066 0%, #FF8C42 100%);
    font-family: 'Hiragino Kaku Gothic ProN', 'Meiryo', sans-serif;
  }

  .card {
    background: rgba(255, 255, 255, 0.85);
    border-radius: 24px;
    padding: 48px 40px;
    text-align: center;
    box-shadow: 0 8px 32px rgba(255, 140, 66, 0.25);
    max-width: 360px;
    width: 90%;
  }

  .avatar {
    width: 160px;
    height: 160px;
    border-radius: 50%;
    object-fit: cover;
    border: 4px solid #FF8C42;
    margin-bottom: 24px;
    background: #FFE8D0;
  }

  .nickname {
    font-size: 2rem;
    font-weight: 700;
    color: #333;
    margin-bottom: 12px;
    letter-spacing: 0.05em;
  }

  .message {
    font-size: 1rem;
    color: #666;
    line-height: 1.6;
  }

  @media (max-width: 480px) {
    .card {
      padding: 36px 24px;
    }

    .avatar {
      width: 120px;
      height: 120px;
    }

    .nickname {
      font-size: 1.6rem;
    }
  }
  ```

- [ ] **Step 2: ブラウザで確認する（PC）**

  `index.html` をブラウザで開き、以下を確認する。
  - 黄〜オレンジのグラデーション背景が表示される
  - 白いカードが中央に表示される
  - イラスト枠が円形で表示される（画像がなければ背景色の丸が出る）
  - ニックネームが大きく、メッセージが小さく表示される

- [ ] **Step 3: ブラウザで確認する（スマホ幅）**

  ブラウザの開発者ツール（F12）でモバイル表示（幅375px相当）に切り替え、レイアウトが崩れないことを確認する。

- [ ] **Step 4: Commit**

  ```bash
  git add style.css
  git commit -m "feat: add CSS styling with gradient background and card layout"
  ```

---

### Task 4: AIイラストを差し込んでコンテンツを確定する

**Files:**
- Modify: `images/avatar.png`（AI生成画像に差し替え）
- Modify: `index.html`（ニックネーム・メッセージを本番テキストに書き換え）

**Interfaces:**
- Consumes: ユーザーが用意したAI生成イラスト画像

- [ ] **Step 1: AI生成イラストを `images/avatar.png` として保存する**

  任意のAI画像生成サービス（Adobe Firefly、Canva、ChatGPT など）でイラストを生成し、`images/avatar.png` として保存する。

- [ ] **Step 2: `index.html` のテキストを本番テキストに書き換える**

  `index.html` の以下の部分を実際のニックネーム・メッセージに変更する。

  ```html
  <!-- 変更前 -->
  <h1 class="nickname">ニックネーム</h1>
  <p class="message">大学×ICTが好きです</p>

  <!-- 変更後（例） -->
  <h1 class="nickname">こたに</h1>
  <p class="message">大学×ICTが好きです ☀️</p>
  ```

- [ ] **Step 3: ブラウザで最終確認する**

  `index.html` をブラウザで開き、AIイラストが円形に表示され、ニックネームとメッセージが正しく表示されることを確認する。スマホ幅でも確認する。

- [ ] **Step 4: Commit**

  ```bash
  git add index.html images/avatar.png
  git commit -m "feat: add AI illustration and finalize content"
  ```

---

## 完成チェックリスト

- [ ] グラデーション背景（黄〜オレンジ）が表示される
- [ ] AIイラストが円形にトリミングされて中央に表示される
- [ ] ニックネームが大きなフォントで表示される
- [ ] 一言メッセージが表示される
- [ ] PC・スマホ両方でレイアウトが崩れない
- [ ] 個人情報（実名・会社名・連絡先）が含まれていない
