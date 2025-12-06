# Doc-React-Typescript-make-Notion-clone

## タイトル

React + Typescriptで人気アプリ「Notion」のクローンを作る講座。ログイン/ユーザー登録機能、入れ子でのノートの作成、リアルタイム通信など、本格的なWebアプリの機能開発が学べます。

## 製作者

くるしば

## この講座で学べること

- React + TypeScript + Supabase を使った本格的な開発
- 基本的なCRUD、検索、リアルタイム通信などの機能
- Jotaiを使ったグローバル状態管理
- React + Tailwind CSS（v4）のセットアップ方法

## この講座の対象者

- Reactの基礎学習をおえた方
- エンジニアとしての就職を目指している方
- Reactをより深く理解したい方

## 受講頂く上での注意事項

- 基本的にPCを推奨します。
- 作ったアプリはカスタマイズしましょう。
- 受講終了後に、レビューをお願いします。

## アプリの機能要件

- ノートのタイトル/本文を作成、編集、削除（CRUD）できる
- ノートのスタイル（フォントなど）を変更することができる
- ノートはリアルタイムで共同編集ができる編集ができる
- テキストはドラッグアンドドロップで移動することができる
- 左側メニューにノート一覧が表示される
- ノートは検索することができる
- アカウントの登録、ログイン、ログアウト機能
- 質問Bot機能（AIによる文書解析、概要生成など、Google NotebookLMのようなRAG機能を想定）
- 学習用データを登録できる機能

## 1. 開発環境のセットアップ

### 1. Node.jsのインストール

macOSを利用する場合、ターミナルを起動してバージョンを確認しましょう。

```bash
node -v
```

v22.18.0

バージョンが表示されない場合は、[https://nodejs.org/ja/download](https://nodejs.org/ja/download)よりインストールしてください。

### 2. テキストエディターをインストールします

[https://code.visualstudio.com/](https://code.visualstudio.com/)

### 3. おすすめの拡張機能

- ES7 + React/Redux/React-Native snippets

[https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

- Auto Close Tag

[https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)

- Emmet（VS Codeに標準搭載）

### 4. プロジェクトの初期設定

ReactとTypescriptの環境をVite経由でセットアップします。

### 5. プロジェクトディレクトリを作成する

```sh
cd ~ && mkdir -p work/Udemy && cd $_
```

### 6. プロジェクトを作成する

```sh
npm create vite@latest notion-clone-app
```

```sh

│
◆  Select a framework:
│  ● React
└

◆  Select a variant:
│  ● TypeScript
└

◆  Use rolldown-vite (Experimental)?:
│  ● No
└

◆  Install with npm and start now?
│  ○ Yes / ● No
└
```

```sh
cd notion-clone-app
```

### 7. プロジェクトにパッケージをインストール

1.cmdk

キーボード操作に特化したコマンドパレットインターフェースを作成するためのライブラリです。

```sh
npm install cmdk
```

2. lucide-react

React用のアイコンコンポーネント集です。

```sh
npm install lucide-react
```

3. @radix-ui/react-dropdown-menu

Radix UI の「ドロップダウンメニュー」コンポーネントを React で使うためのパッケージです。

```sh
npm install @radix-ui/react-dropdown-menu
```

4. Tailwind CSS

- Tailwind CSS　インストール

npm経由でtailwindcssと@tailwindcss/viteをインストールします。

```sh
npm install tailwindcss @tailwindcss/vite
```

- Viteプラグインの設定

プロジェクト直下にある、vite.config.ts　ファイルに、@tailwindcss/viteプラグインを追加してください。

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```








```

すると開発サーバーが起動されます。

[http://localhost:5173/](http://localhost:5173/)　をブラウザーで表示するとReactの初回画面が表示されます。

プロジェクトの雛形ができました。

開発サーバーが起動していたら contrl + C　で停止させます。

### 7. npmのパッケージのインストール

必要なパッケージをまとめてインストールします。

```bash
cd notion-clone-app
```





```sh
npm install tailwindcss@next @tailwindcss/vite@next
```


4. tailwind-merge

[https://www.npmjs.com/package/tailwind-merge](https://www.npmjs.com/package/tailwind-merge)

```sh
npm i tailwind-merge
```

5. tailwindcss-animate

[https://www.npmjs.com/package/tailwindcss-animate](https://www.npmjs.com/package/tailwindcss-animate)

```sh
npm install tailwindcss-animate
```

6. postcss

[https://www.npmjs.com/package/postcss](https://www.npmjs.com/package/postcss)

```sh
npm install -D postcss
```


8. autoprefixer

[https://www.npmjs.com/package/autoprefixer](https://www.npmjs.com/package/autoprefixer)

```sh
npm install -D autoprefixer
```

### npm install

パッケージを node_modules にインストールするには npm install とコマンド実行します。

```sh
% npm install
```

### 開発サーバーで確認

```
% npm run dev
```

[http://localhost:5173/](http://localhost:5173/)　を開くと Reactの初期画面が表示されます。

停止するには、control + c で開発サーバを停止します。

プロジェクトにコンポーネントの見た目を追加したあと。

Reactのロジックを作成していきます。


### tailwindcss のViteプラグインを設定する

Viteの設定に@tailwindcss/viteプラグインを追加してください。

プロジェクトディレクトリ直下にある、vite.config.ts ファイルを編集します。

```vite.config.ts 
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

### Import Tailwind CSS

CSSファイルに@importを追加し、Tailwind CSSをインポートしてください。

reactでは、src/App.css 、src/index.cssファイルに、cssを記載しています。
src/index.cssは、reset.cssなどのグローバルスタイルを記載します。
一方、src/App.cssファイルは、Appコンポーネントに関連するスタイルを掲載します。

なので、src/index.cssの中身を消して、以下の内容を掲載します。

```ts
@import "tailwindcss";
```

### テストしてみましょう

src/App.tsx ファイルを編集します。

```tsx
<h1>Vite + React</h1>
```

を

```tsx
<h1 className="text-3xl font-bold text-blue-600">Vite + React</h1>
```

に変更して保存します。

開発サーバーを起動してブラウザーで確認しましょう。



tailwind.config.js

tsconfig.app.json

tsconfig.json



をプロジェクト直下に作成します。




