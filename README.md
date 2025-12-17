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

ReactとTypeScriptの環境をVite経由でセットアップします。

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

4. Tailwind CSSのインストール

- npm経由でtailwindcssと@tailwindcss/viteをインストールします。

```sh
npm install tailwindcss @tailwindcss/vite
```

- CSS ファイルに @import を追加し、Tailwind CSS をインポートします。

reactのプロジェクトには、src/App.css と、src/index.css にcssを設定するファイルがあります。 
src/App.css はコンポーネントのCSSを記述し、src/index.cssはreset.cssなどプロジェクト全体に適用したいCSSを書きます。

src/index.css 内のすべてを以下で置き換えてください。

```src/index.css
@import "tailwindcss";
```

- tsconfig.json ファイルを編集する

現在のバージョンのViteでは、TypeScriptの設定が3つのファイルに分割されており、そのうち2つを編集する必要があります。プロジェクト直下にある、tsconfig.jsonとtsconfig.app.jsonファイルのcompilerOptionsセクションにbaseUrlとpathsプロパティを追加してください。

```tsconfig.json
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

- tsconfig.app.json ファイルを編集する

以下のコードをプロジェクト直下にある、tsconfig.app.jsonファイルに追加して、IDE用のパスを解決してください。

```tsconfig.app.json
{
  "compilerOptions": {
    /* 省略 */
    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedSideEffectImports": true,
    /* ,を忘れずに、以下を追加  */
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

- vite.config.tsを更新する

以下のコードをプロジェクト直下にある、 vite.config.ts に追加し、アプリがエラーなくパスを解決できるようにします。

```bash
npm install -D @types/node
```

```vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

5. shadcn/ui をインストールする

shadcn/uiは、React製のUIライブラリである

プロジェクトを設定するには、shadcn init コマンドを実行してください。

```sh
npx shadcn@latest init
```

以下の内容が表示される場合がありますが。
次のパッケージをインストールする必要があります。
表示されたらyキーエンターでインストールしてください。

```sh
Need to install the following packages:
shadcn@3.5.2
Ok to proceed? (y)
```

コンポーネント構成ファイル `components.json` を設定するため、いくつかの質問が表示されます。
Neutralを選択してエンターキーでインストールしてください。

```sh
? Which color would you like to use as the base color? › - Use arrow-keys. Return to submit.
❯   Neutral
```

Success! Project initialization completed.

- shadcn/ui を試してみましょう！

Componentsを追加します。

```sh
npx shadcn@latest add button
```

上記のコマンドを実行すると、Buttonコンポーネントがプロジェクトに追加されます。その後、次のようにインポートできます。

src/App.tsxファイルを編集します。

```
import { Button } from "@/components/ui/button";

function App() {
  return (
    <div className="flex min-h-svh flex-col items-center justify-center">
      <h1 className="font-bold text-2xl text-blue-500">
        Vite + React + tailwindcss + shadcn/uiのサンプル
      </h1>
      <Button>Click me</Button>
    </div>
  );
}

export default App;

```

に変更して、開発サーバーを起動して試してみましょう。

6. tailwind-merge をインストール

Tailwind CSS のクラス名の競合（上書き）を自動で解決してくれるユーティリティ関数を提供するパッケージです。

```sh
npm i tailwind-merge
```

6. tailwindcss-animate

Tailwind CSS にアニメーション

```sh
npm install tailwindcss-animate
```

7. react-router-dom をインストールします

ページ遷移（ルーティング）の仕組みを提供してくれるライブラリです。

```bash
npm install react-router-dom
```

8. react-textarea-autosizeをインストールします。

入力内容に応じて高さが自動で伸び縮みするtextareaタグを簡単に実装できるReactライブルラリ

```bash
% npm i react-textarea-autosize
```

- プロジェクトに必要なパッケージ（依存関係）をインストールする

```sh
npm install
```

- 開発サーバーの起動

```sh
npm run dev
```

すると開発サーバーが起動されます。

[http://localhost:5173/](http://localhost:5173/)　をブラウザーで表示するとアプリが表示されます。

プロジェクトの雛形ができました。

開発サーバーが起動していたら contrl + C　で停止させます。

## ルーティングを設定しよう

Notion Clone のプロダクトは、

アカウント認証
登録
フォーム
ノート詳細

の4っつの要素から成り立っています。

4っつの要素つまり、4っつの画面にURLを割り当てます。

## React Routerのコンポーネントを記述する

- src/App.tsx を修正します。

```tsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Layout from "./components/Layout";
import Home from "./pages/Home";
import NoteDetail from "./pages/NoteDetail";
import Signin from "./pages/Signin";
import Signup from "./pages/Signup";

function App() {
  return (
    <BrowserRouter>
      <div className="h-full">
        <Routes>
          {/* Layoutコンポーネント */}
          <Route path="/" element={<Layout />}>
            <Route index element={<Home />} />
            <Route path="/notes/:id" element={<NoteDetail />} />
          </Route>
          <Route path="/signin" element={<Signin />} />
          <Route path="/signup" element={<Signup />} />
        </Routes>
      </div>
    </BrowserRouter>
  );
}

export default App;

```

Routeタグの中身が重要で、それぞれのコンポーネント名と、URLがひもづきを設定しています。



### layout.ts

このままでは、ルーティング先のファイルがないのでエラーになります。続いて`src/Layout.tsx`ファイルを作成します。

プロジェクト直下で

```sh
touch src/Layout.tsx
```

```tsx
import { Outlet } from "react-router-dom";

const Layout = () => {
  return (
    <div>
      <h1>Layoutを表示します。</h1>
      <Outlet />
    </div>
  );
};

export default Layout;
```




