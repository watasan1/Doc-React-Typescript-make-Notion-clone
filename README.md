# Doc-React-Typescript-make-Notion-clone

## 1. 講座概要

### 1.1 タイトル

React + TypeScriptで人気アプリ「Notion」のクローンを作る講座。
認証機能、入れ子構造のノートの作成、リアルタイム通信など、本格的なWebアプリ開発を通じて実践力を身につけます。

### 1.2 製作者

くるしば

### 1.3 この講座で学べること

- React + TypeScript + Supabase を使った本格的なWebアプリ開発
- ノートアプリを題材にしたCRUD/検索/リアルタイム通信などの実装
- Jotaiを使ったシンプルなグローバル状態管理
- React + Tailwind CSS（v4）のセットアップと実践的な使い方

### 1.4 この講座の対象者

- Reactの基礎学習を終えた方
- エンジニアとしての就職・転職をを目指している方
- React／TypeScriptを使った実践的なアプリ開発を学びたい方

### 1.5 受講にあたっての注意点

- 学習はPCの利用を推奨します。
- 講座内で作成したアプリは、ぜひデザインや機能をカスタマイズしてみてください。
- 受講終了後にレビューをいただけると励みになります。

## 2. アプリの機能要件

### 2.1 基本機能

- ノートのタイトル/本文を作成・編集・削除できる（CRUD）
- 入れ子構造（階層構造）のノートを作成できる
- 左側メニューにノート一覧表示
- ノートを検索機能
- アカウントの認証機能

### 2.2 応用機能

- ノートをリアルタイムで共同編集
- ドラッグ&ドロップによるノート移動
- ノートスタイル（フォントなど）の変更

### 2.3 発展機能（チャレンジ）

- 質問Bot機能
  - AIによる文書解析、概要生成など（Google NotebookLMのようなRAG機能を想定）
- 学習用データを登録機能

## 3. 開発環境のセットアップ

### 3.1 Node.jsのインストール

macOSを利用する場合、ターミナルを起動して以下のコマンドを実行してください。

```bash
node -v
```

Node.js v18 以上（LTS 推奨）が表示されれば問題ありません。

バージョンが表示されない場合、公式サイトからインストールしてください。

[https://nodejs.org/ja/download](https://nodejs.org/ja/download)

### 3.2 テキストエディターのインストール

Visual Studio Code をインストールします。

[https://code.visualstudio.com/](https://code.visualstudio.com/)

### 3.3 おすすめのVSCode拡張機能

- ES7 + React/Redux/React-Native snippets

[https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

- Auto Close Tag

[https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)

- Emmet
  VS Codeに標準搭載

### 3.4 プロジェクトの初期設定方針

本講座ではViteを使用して、React + TypeScriptの開発環境を構築します。

### 3.5 プロジェクトディレクトリを作成

```bash
cd ~ && mkdir -p work/Udemy && cd $_
```

### 3.6 プロジェクトを作成する

以下のコマンドを実行して、新しいReactプロジェクトを作成します。

```bash
npm create vite@latest notion-clone-app
```

対話形式で以下を選択してください。

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

プロジェクト作成後、ディレクトリに移動します。

```bash
cd notion-clone-app
```

## 4. パッケージのインストールと初期設定

### 4.1.1 この章について

この章では、アプリ開発に必要な npm パッケージ をインストールします。
本章では、役割に応じて以下のように呼び分けます。

- ライブラリ：単体で機能を提供する
- プラグイン：特定のツールを拡張するための

### 4.2 Tailwind CSSのセットアップ

Vite用の公式プラグイン（`@tailwindcss/vite`）を含めてインストールします。

```bash
npm install tailwindcss @tailwindcss/vite
```

- CSS ファイルの設定

React（Vite）プロジェクトでは、主に次の2つのCSSファイルがあります。

- `src/index.css`：リセットCSSやTailwindCSSなど、全体に適用するスタイル
- `src/App.css`：コンポーネント固有のスタイル

`src/index.css`の中身をすべて削除して、以下の内容に置き換えてください。

```src/index.css
@import "tailwindcss";
```

### 4.3 パスエイリアス（@/）の設定

`tsconfig.json` と `tsconfig.app.json` の両方に `baseUrl` と `paths` を設定します。

これにより、以下のような import が可能になります。

```tsx
import Header from "@/components/Header";
```

パスエイリアスを設定すると、`@`を使って`src`配下のファイルを参照できるようになります。

- tsconfig.json ファイルを編集する

ViteではTypeScriptの設定が複数ファイルに分かれています。

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

IDE（エディタ）で正しくパスを解決できるように設定を追加します。

以下のコードをプロジェクト直下にある、tsconfig.app.jsonファイルに追加します。

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

4.4 vite.config.tsを更新する

- Node.jsの型定義をインストール

```bash
npm install -D @types/node
```

- vite.config.ts を編集する

```vite.config.ts
import tailwindcss from "@tailwindcss/vite";
import react from "@vitejs/plugin-react";
import path from "path";
import { defineConfig } from "vite";

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
});

```

4.5 shadcn/ui の導入

shadcn/ui は、Radix UI と Tailwind CSS をベースにした実務向けの UI コンポーネント集です。

※ shadcn/ui は npm install ではなく、CLI を使って
プロジェクト構成に合わせたファイルを直接生成します。

```bash
npx shadcn@latest init
```

途中で以下の表示が出た場合は、`y`を入力して進めてください。


```bash
Need to install the following packages:
shadcn@3.5.2
Ok to proceed? (y)
```

コンポーネント構成ファイル `components.json` を設定するため、カラーテーマを選択する質問が表示されます。
Neutralを選択してエンターキーでインストールしてください。

```sh
? Which color would you like to use as the base color? › - Use arrow-keys. Return to submit.
❯   Neutral
```

選択後、以下が表示されれば成功です。

```text
Success! Project initialization completed.
```

## 5. UI・構造系ライブラリの導入

### 5.1 UI関連ライブラリ

#### 5.1.1 cmdkをインストール（ライブラリ）

cmdkは、キーボード操作に特化したコマンドパレットUIを実装できるReactライブラリです。

```bash
npm install cmdk
```

#### 5.1.2 lucide-react をインストール（ライブラリ）

lucide-reactは、React用のアイコンコンポーネント集です。
shadcn/ui と相性が良いのが特徴です。

```bash
npm install lucide-react
```

#### 5.1.3 @radix-ui/react-dropdown-menuをインストール(ライブラリ)

Radix UI のドロップダウンメニューコンポーネントをReactで利用するためのパッケージです。

```bash
npm install @radix-ui/react-dropdown-menu
```

#### 5.1.4 tailwind-merge をインストール（ライブラリ）

Tailwind CSS のクラス名の競合を自動で解決してくれます。

```sh
npm i tailwind-merge
```

#### 5.1.5 tailwindcss-animateをインストール（プラグイン）

Tailwind CSS にアニメーション用のユーティリティを追加します。

```bash
npm install tailwindcss-animate
```

### 5.2 アプリ構造系ライブラリ

#### 5.2.1 react-router-dom をインストールします（ライブラリ）

ページ遷移（ルーティング）を実現するReactライブラリです。

```bash
npm install react-router-dom
```

#### 5.2.2 react-textarea-autosizeをインストール（ライブラリ）

入力内容に応じて高さが自動で変化するtextareaを実装できるReactライブラリです。

```bash
npm i react-textarea-autosize
```

## 6. Prettierの導入

Prettierは、コードの整形（フォーマット）を自動で行うツールです。

### 6.1 Prettierのパッケージのインストール

- `prettier-plugin-tailwindcss`は、Tailwind CSSのclassの順番をルールにしたがって並べ変えるプラグインです。

- `@trivago/prettier-plugin-sort-imports`は、import 文の順番をルール通りに整列するプラグインです。

```Bash
npm install -D prettier prettier-plugin-tailwindcss @trivago/prettier-plugin-sort-imports
```

### 6.2 Prettierの設定ファイルを作成する

プロジェクト直下に.prettierrcファイルを作成する

```bash
touch .prettierrc
```

```json
{
  "plugins": [
    "@trivago/prettier-plugin-sort-imports",
    "prettier-plugin-tailwindcss"
  ],
  "importOrder": [
    "^react$",
    "^react-dom$",
    "^@?\\w",
    "^@/components/(.*)$",
    "^@/hooks/(.*)$",
    "^@/lib/(.*)$",
    "^[./]"
  ],
  "importOrderSeparation": false,
  "importOrderSortSpecifiers": true,
  "importOrderParserPlugins": ["typescript", "jsx"]
}

```

### 6.3 Prettierを実行する

```bash
npx prettier --write .
```

## 7. shadcn/ui を試す

Button コンポーネントを追加します。

```bash
npx shadcn@latest add button
```

上記のコマンドを実行すると、Buttonコンポーネントがプロジェクトに追加されます。その後、次のようにインポートできます。

`src/App.tsx`ファイルを編集します。

```src/App.tsx
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

## 8. 開発サーバーの起動する

```bash
npm run dev
```

[http://localhost:5173/](http://localhost:5173/)にアクセスして表示を確認してください。

開発サーバーを停止する場合は、Ctrl + C　を押してください。

これでプロジェクトの雛形が完成しました。

## 9. ルーティングを設定しよう

Notion Clone のプロダクトは、主に次の4つの画面から成り立っています。

- トップページ(Home)
- サインイン(Signin)
- サインアップ(Signup)
- ノート詳細(NoteDetail)

これら、4つの画面にそれぞれにURLを割り当てて表示を切り替えるために、React Routerを使ってルーティングを設定していきます。

### 9.1 React Router のコンポーネントを記述します。

`src/App.tsx` は、アプリ全体の画面構成とURLの対応関係を定義するファイルです。

- どのURLにアクセスしたとき
- どのReactコンポーネントを表示するのか

を、ここで設定します。

- src/App.tsx を修正します。

```tsx
import { BrowserRouter, Route, Routes } from "react-router-dom";
import Layout from "./Layout";
import Home from "./pages/Home";
import Signin from "./pages/Signin";
import Signup from "./pages/Signup";
import NoteDetail from "./pages/notes/NoteDetail";

function App() {
  return (
    <BrowserRouter>
      <div className="h-full">
        <Routes>
          {/* 共通レイアウト */}
          <Route path="/" element={<Layout />}>
            {/* トップページ（/） */}
            <Route index element={<Home />} />
            {/* ノート詳細ページ(notes/:id) */}
            <Route path="notes/:id" element={<NoteDetail />} />
          </Route>
          {/* 認証関連ページ */}
          <Route path="/signin" element={<Signin />} />
          <Route path="/signup" element={<Signup />} />
        </Routes>
      </div>
    </BrowserRouter>
  );
}

export default App;

```

### src/App.tsxの解説（React Ruter）

### BrowserRouterについて

`BrowserRouter`は、ブラウザのURL（履歴）を管理し、URLの変更に応じて表示する画面を切り替えるための React Router の最上位コンポーネントです。

* Route
* Routes
* useParams
* useNavigate

このコンポーネントの中に記述されたコンポーネントのみが、 React Ruter の機能を利用できます。

BrowserRouterタグでアプリ全体を囲みます。

### Routes について

`Routes`は、現在のURLに一致する`Route`を検索し、その中から最も適切な1つだけを描画するコンポーネントです。

React Router では、`Route`は必ず`Routes`の中に記述する必要があります。

### Route コンポーネントの役割

`Route`コンポーネントでは、URLと表示するReactコンポーネントの対応関係を定義します。

主に次の2つのpropsを使用します。

* `path`: URLのパス

* `element`: そのURLで表示するReactコンポーネント

```tsx
<Route path="/" element={<Layout />}>
```

`element`には、コンポーネントをJSX（<Component />）として直接渡す点がReact Routerの特徴です。

### 共通レイアウト（ネストルーティング）

```tsx
<Route path="/" element={<Layout />}>
```

このように`Route`をネスト（入れ子）させることで、共通レイアウトを持つページ構成を作ることができます。

React Routerにおける「ネスト」とは、Routeを親子関係で定義することを指します。

* 親ルート：共通レイアウト（ヘッダー・フッターなd）
* 子ルート：レイアウト内で切り替わるページ

### Outletについて

ネストされたルートは、`Layout`コンポーネント内にある`<Outlet />`の位置に表示されます。

```tsx
<Outlet />
```

この`<Outlet />`が、「URLに応じて切り替わる表示エリア」になります。

### index ルートについて（トップページ）

```tsx
<Route index element={<Home />} />
```

`index`を指定したルートは、親ルートのパスに完全一致したときに表示されます。

この場合：

* `/`にアクセスしたとき → `Home`コンポーネントが表示されます。

### 表示の仕組み

`Layout` コンポーネントが外枠として存在し、

* Header
* Footer

などの共通パーツを表示します。

その中に`<Outlet />`に、URLに応じて以下が切り替わります。


* `/` のとき → `Home` が表示される

* `/notes/1` のとき → `NoteDetail` が表示される

つまり、「Layout が外枠として存在し、その中の `<Outlet />` だけが切り替わるという構造になっています。

### 動的ルーティングについて

```tsx
<Route path="notes/:id" element={<NoteDetail />} />
```

`:id` は 動的パラメータです。

これにより、次のようなURLに対応できます。

* `/notes/1`
* `/notes/abc`

`NoteDetail`コンポーネント内では、`useParams()`を使ってURLパラメータを取得できます。

```ts
const { id } = useParams();
```

※ `useParams()`で取得できる値は文字列として返されます。
数値として扱いたい場合は、変換が必要です。

### まとめ

* BrowserRouter：URL（履歴）を管理する最上位コンポーネント
* Routes：URL に一致する Route を 1 つ選んで描画
* Route：URL と表示コンポーネントの対応を定義
* ネスト + Outlet：共通レイアウトを実現
* index：トップページ用のルート
* :id：動的ルーティング

### 9.2 layout.tsx

layout.tsx は「どのページでも共通で表示される枠組み」を書くファイルです。

layoutコンポーネントは、ペースの見た目となります。Layoutコンポーネントファイルは、<Outlet />タグにApp.tsx でのpath="/"を引数として渡してあげるとHomeコンポーネントが入ります。

<Route path="/notes/:id" element={<NoteDetail />} />解説してください。


src/App.tsxだけでは、ルーティング先のファイルがないのでエラーになります。続いて`src/Layout.tsx`ファイルを作成します。

プロジェクト直下で

```sh
touch src/Layout.tsx
```

```tsx
import { Outlet } from "react-router-dom";

const Layout = () => {
  return (
    <main className="min-h-screen">
      <h1>Layoutが表示されています。</h1>
      <Outlet />
    </main>
  );
};

export default Layout;

```

Layout.tsx解説

`<Outlet />`ここに、今のURLに対応した子ページを表示してくださいという意味です。

例えば、URL が`/`のとき

Layout.tsx （共通レイアウト）がHomeが入ります。

一方、URLが、`notes/1`のとき

Layout.tsx （共通レイアウト）がHomeが入ります。ノートの1番目の投稿が入ります。






## Home.tsxコンポーネントの作成

```bash
mkdir -p src/pages && touch src/pages/Home.tsx
```

pagesディレクトリは、ルーティング単位の画面コンポーネントを置く場所です。
Home.tsx はトップページに対応する画面になります。

src/App.tsx で、react-router-domを使う場合は、

```App.tsx
<Route index element={<Home />} />
```

のように指定します。

## Home.tsx(トップページ)を作成する

```Home.tsx
const Home = () => {
  return (
    <div>
      <h1>ここがトップページです</h1>
    </div>
  );
}

export default Home;
```

http://localhost:5173/


## pages/notes/NoteDetail.tsx コンポーネントを作成する

```bash
mkdir -p src/pages/notes && touch src/pages/notes/NoteDetail.tsx
```

```pages/notes/NoteDetail.tsx
import { useParams } from "react-router-dom";

const NoteDetail = () => {
  const { id } = useParams<{ id: string }>();
  return <div>ノートID: {id}</div>;
};

export default NoteDetail;

```

http://localhost:5173/notes/1

### ./pages/Signin コンポーネントを作成します。

```bash
touch src/pages/Signin.tsx
```

```pages/Signin.tsx
const Signin = () => {
  return (
    <div>
      Signin
    </div>
  );
};

export default Signin;
```

http://localhost:5173/signin

### ./pages/Signup.tsx コンポーネントを作成します。

```bash
touch src/pages/Signup.tsx
```

```pages/Signup.tsx
const Signup = () => {
  return (
    <div>
      Signup
    </div>
  );
};

export default Signup;
```

http://localhost:5173/signup


