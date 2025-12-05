# Doc-React-Typescript-make-Notion-clone

## タイトル

React + Typescriptで人気アプリ「Notion」のクローンを作る講座。ログイン/ユーザー登録機能、入れ子でのノートの作成、リアルタイム通信など、本格的なWebアプリの機能開発が学べる

## 製作者

くるしば

## この講座で学べること

・React + TypeScript + Supabaseを使った本格的な開発
・基本的なCRUD、検索、リアルタイム通信などの機能
・Jotaiを使ったグローバル状態管理
・React + Tailwind CSSのセットアップなど

## この講座の対象者

・Reactの基礎学習をおえた方
・エンジニアとしての就職を目指している方
・Reactをより深く理解したい方

## 受講頂く上でのお願い

・基本的にPCを推奨
・作ったアプリはカスタマイズしよう
・受講終了後に、レビューをお願いします

## アプリの機能要件

ノートのタイトルを作成することができる
ノートを作成することができる
ノートは親子関係のノートを作成することができる
ノートのスタイルを変更することができる
ノートは削除することができる
ノートはリアルタイム編集ができる
テキストはドラッグアンドドロップで移動することができる

左側メニューにノート一覧が表示される
ノートは検索することができる

ノートアプリは、アカウントの登録、アカウントのログイン、ログアウトができる

質問Bot機能を追加することができ、Google のNotebookLM は、AI搭載のノートリサーチツールで、アップロードした文章や、ウェブ上の情報を解析して、概要を自動生成してくれます。

学習するデータを登録することができます。

## 開発環境を作成する

### Node.jsのインストール

macOSを利用する場合、ターミナルを起動して確認してましょう。

% node -v
v22.18.0

バージョンが表示されなかったら、https://nodejs.org/ja よりインストールしてください

## テキストエディターをインストールします

https://code.visualstudio.com/

## おすすめの拡張機能 tool

### ES7+ React/Redux/React-Native snippets
https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets

### Auto Close Tag
https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag

### Emmet

### テキストの比較

https://www.diffchecker.com/

### 翻訳

deepl

## プロジェクトのセットアップ

ReactとTypescriptの環境をBEAT経由でセットアップする

### プロジェクトディレクトリを作成する

```sh
% cd ~ && mkdir -p work/Udemy && cd $_
```

### プロジェクトを作成する

```sh
% npm create vite@latest
```

```sh
│
◆  Project name:
│  notion-clone-app█

◆  Select a framework:
│  ● React

◆  Select a variant:
│  ● TypeScript

◆  Use rolldown-vite (Experimental)?:
│  ● Yes (The future default Vite, which is powered by Rolldown)

◆  Install with npm and start now?
│  ● Yes / ○ No

```

プロジェクトの雛形ができました。

開発サーバーが起動していたら contrl + C　で停止させます。

### package.jsonにパッケージを追加する

1.cmdk

[https://github.com/dip/cmdk](https://github.com/dip/cmdk)

```sh
npm install cmdk
```

2. lucide-react

[https://lucide.dev/](https://lucide.dev/)

```sh
npm install lucide-react
```

3. @radix-ui/react-dropdown-menu

[https://www.radix-ui.com/primitives/docs/components/dropdown-menu](https://www.radix-ui.com/primitives/docs/components/dropdown-menu)

```sh
npm install @radix-ui/react-dropdown-menu
```

4. tailwind-merge

[https://www.npmjs.com/package/tailwind-merge](https://www.npmjs.com/package/tailwind-merge)

```sh
npm i tailwind-merge
```

5. tailwindcss-animate

[https://www.npmjs.com/package/tailwindcss-animate](https://www.npmjs.com/package/tailwindcss-animate)

```sh
% npm install tailwindcss-animate
```










### 開発サーバーで確認

http://localhost:5173/　を開くと

## TailwindCSS をインストールします。





