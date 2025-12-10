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

Viteを使用して新しいReactプロジェクトを始めます。

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

- Tailwind CSS をインポートする

CSS ファイルに @import を追加し、Tailwind CSS をインポートします。

reactのプロジェクトには、src/App.css と、src/index.css にcssを設定するファイルがあります。 
src/App.css はコンポーネントのCSSを記述し、src/index.cssはreset.cssなどプロジェクト全体に適用したいCSSを書きます。

なので、src/index.css の内容を消して、以下の内容を掲載します。

```src/index.css
@import "tailwindcss";
```

- TailwindCSSを試す。

src/App.tsx ファイルの`<h1>Vite + React</h1>` タグをTailwindCSSに変更します。

```src/App.tsx
<h1 className="text-3xl font-bold underline">Vite + React</h1>
```

に変更して、開発サーバーを起動して試してみましょう。

- 開発サーバーの起動

```sh
npm run dev
```

すると開発サーバーが起動されます。

[http://localhost:5173/](http://localhost:5173/)　をブラウザーで表示するとReactの初回画面が表示されます。

タイトルにTailwindCSSが導入されたことを確認します。

プロジェクトの雛形ができました。

開発サーバーが起動していたら contrl + C　で停止させます。

5. tailwind-merge

Tailwind CSS のクラス名の競合（上書き）を自動で解決してくれるユーティリティ関数を提供するパッケージです。

```sh
npm i tailwind-merge
```

6. tailwindcss-animate

Tailwind CSS にアニメーション

```sh
npm install tailwindcss-animate
```

### 7. shadcn/ui をインストールする

shadcn/uiは、React製のUIライブラリである

### 8. CLIを実行する

### 9. npm install

パッケージを node_modules にインストールするには npm install とコマンド実行します。

```sh
% npm install
```

### 10. 開発サーバーで確認

```
% npm run dev
```

[http://localhost:5173/](http://localhost:5173/)　を開くと Reactの初期画面が表示されます。

停止するには、control + c で開発サーバを停止します。

### 10. tsconfig.json を編集します。

tsconfig.jsonとtsconfig.app.jsonファイルのcompilerOptionsセクションにbaseUrlとpathsプロパティを追加してください。

```
{
  "files": [],
  "references": [
    { "path": "./tsconfig.app.json" },
    { "path": "./tsconfig.node.json" }
  ],
  /* ,を忘れずに、ここから追加 */
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
  /* ここまで追加 */
}

```



### 11. tsconfig.app.jsonを変更する

IDE のパスを解決するには、次のコードを tsconfig.app.json ファイルに追加します。

```JSONC
{
  "compilerOptions": {
    /* 省略 */
    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedSideEffectImports": true,
    /* 以下を追加  */
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
    /* ここまで追加 */
  },
  "include": ["src"]
}

```

この設定を追加することで、プロジェクト内のファイルを`@`を使って参照できるようになります。

これまで相対パスで書いていた

```ts
import Header from "../../components/Header";
```

という記述が、次のようにシンプルになります。

```ts
import Header from "@/components/Header";
```

これにより、パスの見通しが良くなり、コードの可読性・保守性が大きく向上します。

### 12. vite.config.ts

次に、Vite側でも同じエイリアス設定を行う必要があります。

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'
import path from "node:path";

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
    resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
})

```

これで、プロジェクト全体で、`@/`を使ったパスエイリアスを利用できるようになります。

## CLIを実行する

プロジェクトを設定するには、shadcn init コマンドを実行してください:

```sh
npx shadcn@latest init
```

コンポーネント構成ファイル `components.json` を設定するため、いくつかの質問が表示されます。

```sh
Which color would you like to use as base color? › Neutral
```

Success! Project initialization completed.
You may now add components.

という文字を作成して完了です。


## 雛形を作成する

```sh
├── components
│   ├── Editor.tsx
│   ├── NoteList
│   │   ├── index.tsx
│   │   └── NoteItem.tsx
│   ├── SearchModal.tsx
│   ├── SideBar
│   │   ├── index.tsx
│   │   ├── Item.tsx
│   │   └── UserItem.tsx
│   ├── Toolbar.tsx
│   └── ui
│       ├── card.tsx
│       ├── command.tsx
│       ├── dialog.tsx
│       ├── dropdown-menu.tsx
│       ├── input.tsx
│       └── popover.tsx
```

```
index.css
```

```
Layout.tsx
```

```
├── lib
│   └── utils.ts
```

```
├── pages
│   ├── Home.tsx
│   ├── NoteDetail.tsx
│   ├── Signin.tsx
│   └── Signup.tsx
```

## assets/react.svg フォルダの中身は不要なので削除する 

```sh
├── src
│   ├── assets
│   │   └── react.svg
```

## ロジックを初期化します。

- src/App.tsx を修正します。

```tsx
function App() {
  return
  <div className="text-2xl text-orange-500 font-bold">Hello, World!</div>;
}

export default App;
```






