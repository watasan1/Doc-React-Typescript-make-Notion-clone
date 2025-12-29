# Doc-React-Typescript-make-Notion-clone

## 1. 講座概要

### 1.1 タイトル

React + TypeScriptで人気アプリ「Notion」のクローンを作る実践講座。
認証機能、入れ子構造のノート作成、リアルタイム通信などを実装し、本格的なWebアプリ開発を通じて実践力を身につけます。

### 1.2 製作者

くるしば

### 1.3 この講座で学べること

- React + TypeScript + Supabase を使った本格的なWebアプリ開発
- ノートアプリを題材にしたCRUD・検索・リアルタイム通信などの実装
- Jotaiを使ったシンプルで扱いやすいグローバル状態管理
- React + Tailwind CSS（v4）のセットアップと実践的な活用方法

### 1.4 この講座の対象者

- Reactの基礎学習を終え、次のステップに進みたい方
- エンジニアとしての就職・転職を目指している方
- React／TypeScriptを使った実践的なアプリ開発を学びたい方

### 1.5 受講にあたっての注意点

- 学習はPCでの利用を推奨します。
- 講座内で作成したアプリは、ぜひデザインや機能をカスタマイズしてみてください。
- 受講終了後にレビューをいただけると、今後の講座改善の励みになります。

### 1.6 アプリの機能要件

#### 1.6.1 基本機能

- ノートのタイトル/本文を作成・編集・削除できる（CRUD）
- 入れ子構造（階層構造）のノートを作成できる
- 左側メニューにノート一覧表示
- ノートの検索
- アカウントの認証機能

#### 1.6.2 応用機能

- ノートをリアルタイムで共同編集
- ドラッグ&ドロップによるノート移動
- ノートスタイル（フォントなど）の変更

#### 1.6.3 発展機能（チャレンジ）

- 質問Bot機能
  - AIによる文書解析、概要生成など（Google NotebookLMのようなRAG機能を想定）
- 学習用データを登録機能

## 2. 開発環境のセットアップ

### 2.1 Node.jsのインストール

macOSを利用する場合、ターミナルを起動して以下のコマンドを実行してください。

```bash
node -v
```

Node.js v18 以上（LTS 推奨）が表示されれば問題ありません。

バージョンが表示されない場合、公式サイトからインストールしてください。

[https://nodejs.org/ja/download](https://nodejs.org/ja/download)

### 2.2 テキストエディターのインストール

Visual Studio Code をインストールします。

[https://code.visualstudio.com/](https://code.visualstudio.com/)

### 2.3 おすすめのVSCode拡張機能

- ES7 + React/Redux/React-Native snippets

[https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

- Auto Close Tag

[https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)

- Emmet
  VS Codeに標準搭載

### 2.4 プロジェクトの初期設定方針

本講座ではViteを使用して、React + TypeScriptの開発環境を構築します。

### 2.5 プロジェクトディレクトリを作成

```bash
cd ~ && mkdir -p work/Udemy && cd $_
```

### 2.6 プロジェクトを作成する

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

## 3. プロジェクト初期セットアップ

### 3.1 この章について

この章では、プロジェクトを実際に開発できる状態に整えるための初期セットアップを行います。

具体的には、以下の内容を扱います。

* Tailwind CSSを使ったスタイリング環境の構築
* パスエイリアス(`@/`)の設定
* Viteの設定ファイルの更新
* shadcn/ui の初期化

ここで行う設定は、以降の章すべて土台になります。
1つずつ確実に進めていきましょう。

### 3.2 Tailwind CSSのセットアップ

本講座では、スタイリングにTailwind CSSを使用します。

Vite用の公式プラグイン（`@tailwindcss/vite`）を含めてインストールします。

```bash
npm install tailwindcss @tailwindcss/vite
```

### 3.2.1 CSSファイルの構成について

React（Vite）プロジェクトでは、主に次の2つのCSSファイルがあります。

* `src/index.css`：アプリ全体に適用するスタイル（リセットCSS、TailwindCSSなど）
* `src/App.css`：コンポーネント単位で使われるスタイル

本講座では、スタイル管理を Tailwind CSSに統一するため、`src/index.css`の中身をすべて削除して、以下の内容に置き換えてください。

```src/index.css
@import "tailwindcss";
```

### 3.3 パスエイリアス（@/）の設定

import文が深くなってくると、相対パス(`../../`)は可読性を下げます。
そこで本講座では、`@`を使って`src`配下を参照できるように設定します。

`tsconfig.json` と `tsconfig.app.json` の両方に `baseUrl` と `paths` を設定します。

これにより、以下のような import が可能になります。

```tsx
import Header from "@/components/Header";
```

パスエイリアスを設定すると、`@`を使って`src`配下のファイルを参照できるようになります。

#### 3.3.1 tsconfig.json ファイルを編集する

ViteではTypeScriptの設定が複数ファイルに分かれています。
まず、プロジェクト直下の`tsconfig.json`を編集します。

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

#### 3.3.2 tsconfig.app.json ファイルを編集する

IDE（エディタ）上で正しくパスを解決されるよう、以下のコードをプロジェクト直下にある、`tsconfig.app.json`ファイルにも同様の設定を追加します。

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

### 3.4 vite.config.tsを更新する

#### 3.4.1 Node.jsの型定義をインストール

Viteの設定ファイル内で`path`モジュールを使用するため、Node.jsの型定義を開発依存としてインストールします。

```bash
npm install -D @types/node
```

#### 3.4.2 vite.config.ts を編集する

次に、`vite.config.ts`を編集し、

* Tailwind CSS プラグインの追加
* パスエイリアスの設定

を行います。

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

### 3.5 shadcn/ui の導入

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

## 4. UI・構造系ライブラリの導入

この章では、アプリ全体で使用するUI系・構造系ライブラリをまとめて導入します。

### 4.1 UI関連ライブラリ

#### 4.1.1 cmdk（コマンドパレット）

```bash
npm install cmdk
```

cmdkは、キーボード操作に特化したコマンドパレットUIを実装するためのライブラリです。


#### 4.1.2 lucide-react（ライブラリ）

```bash
npm install lucide-react
```

shadcn/uiと相性の良い、React用のアイコンコンポーネント集です。

#### 4.1.3 @radix-ui/react-dropdown-menu(ライブラリ)

```bash
npm install @radix-ui/react-dropdown-menu
```

Radix UI のドロップダウンメニューコンポーネントを利用するためのライブラリです。

#### 4.1.4 tailwind-merge（ライブラリ）

```sh
npm i tailwind-merge
```

Tailwind CSS のクラス名の競合を自動で解決してくれます。

#### 4.1.5 tailwindcss-animate（プラグイン）

```bash
npm install tailwindcss-animate
```

Tailwind CSS にアニメーション用のユーティリティを追加します。

### 4.2 アプリ構造系ライブラリ

#### 4.2.1 react-router-dom（ライブラリ）

```bash
npm install react-router-dom
```

ページ遷移（ルーティング）を実現するReactライブラリです。

#### 4.2.2 react-textarea-autosize（ライブラリ）

```bash
npm i react-textarea-autosize
```

入力内容に応じて高さが自動で変化するtextareaを実装できます。

## 5. 開発体験を整える

### 5.1 Prettierの導入

Prettierは、コードの整形（フォーマット）を自動で行うツールです。

```bash
npm install -D prettier prettier-plugin-tailwindcss @trivago/prettier-plugin-sort-imports
```

* `prettier-plugin-tailwindcss`は、Tailwind CSSのclassの順番を整列するプラグイン。

* `@trivago/prettier-plugin-sort-imports`は、import 文の順番を整列するプラグイン。

### 5.2 Prettier設定ファイルの作成

プロジェクト直下に`.prettierrc`ファイルを作成します。

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

### 5.3 フォーマットを実行する

以下のコマンドで、プロジェクト全体を整形します。

```bash
npx prettier --write .
```

## 6. 動作確認と初期画面の表示

### 6.1 shadcn/ui を使ってみる

#### 6.1.1 Button コンポーネントを追加します。

```bash
npx shadcn@latest add button
```

#### 6.1.2 `src/App.tsx`ファイルを編集します。

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

#### 6.1.3 動作確認

* 開発サーバーを起動する

```bash
npm run dev
```

ブラウザで以下にアクセスしてください。

[http://localhost:5173/](http://localhost:5173/)

開発サーバーを停止する場合は、Ctrl + C　を押してください。

表示が確認できれば、プロジェクトの雛形は完成です。

## 7. ページ構成とルーティング設計

Notion Clone のアプリには、次の4つの画面があります。

- トップページ(Home)
- サインイン(Signin)
- サインアップ(Signup)
- ノート詳細(NoteDetail)

これらの画面を、URLによって切り替える仕組みを作るためにReact Routerを使ってルーティングを設定していきます。

URLに応じて、どのコンポーネントを表示するかを決める仕組みです。

### 7.1 App.tsxにルーティングを書こう

`src/App.tsx` は、アプリ全体の画面構成とURLの対応関係を定義するファイルです。

画面の見た目ではなく、ページの切り替えルールだけを担当します。

#### 7.1.1 React Routerを使う準備

```tsx
import { BrowserRouter, Route, Routes } from "react-router-dom";
```

| 名前            | 役割                 |
| ------------- | ------------------ |
| BrowserRouter | URL を管理する          |
| Routes        | どの Route を表示するか決める |
| Route         | URL と画面を対応させる      |

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

* src/App.tsxの解説（React Router）

1. BrowserRouterについて

`BrowserRouter`は、ブラウザのURL（履歴）を管理し、URLの変更に応じて表示する画面を切り替えるための React Router のコンポーネントです。
React Routerの機能は、この中に書いたコンポーネントでしか使えません。

2. Routes について

Route コンポーネントの役割

`Route`コンポーネントでは、URLと表示するReactコンポーネントの対応関係を定義します。

`Routes`は、現在のURLに一致する`Route`を検索し、その中からURLに合う`Route`を描画するコンポーネントです。

`Route`は、このURLなら、この画面というルールを書きます。

```tsx
<Route path="/signin" element={<Signin />} />
```

主に次の2つのpropsを使用します。

* `path`: URLのパス

* `element`: そのURLで表示するReactコンポーネント

これは、`signin`にアクセスしたら`Signin`コンポーネントを表示します。

### 共通レイアウトを使ったルーティング

```tsx
<Route path="/" element={<Layout />}>
  <Route index element={<Home />} />
  <Route path="notes/:id" element={<NoteDetail />} />
</Route>

```

ここでは、共通レイアウトを使ったルーティングを設定しています。

### 共通レイアウトとは？

「どのページでも同じ見た目になる部分」です。

例：

* ヘッダー
* フッター
* メニュー

ページごとに変わるのは中身だけ、という構造を作れます。

トップページ（indexルート）

```tsx
<Route index element={<Home />} />
```

`index`は、`/`にアクセスすると`Home`コンポーネントが表示されます。

### ノート詳細ページ（動的ルーティング）

```tsx
<Route path="notes/:id" element={<NoteDetail />} />

```

`:id`は変わる値を表示します。

これにより

* `/notes/1`
* `/notes/2`
* `/notes/abc`

といったURLを、1のルールで扱うことができます。

### 7.2 Layout.tsxを作成する

Layout.tsx はどのページでも共通で表示される「外枠」を書くファイルです。

App.tsx で指定した共通レイアウトの中身を、ここで実際に作ります。

プロジェクト直下で以下のコマンドを実行します。

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

解説

`<Outlet />` は、ここに、今のURLにあったページを表示するという目印になります。

画面が切り替わる仕組み
URL が `/` のとき

* App.tsx
→ `index` ルートが選ばれる

* Layout.tsx
→ `<Outlet />` に Home が表示される

URL が `/notes/1` のとき

* App.tsx
→ `notes/:id` が選ばれる

* Layout.tsx
→ `<Outlet />` に NoteDetail が表示される

大事なポイント

* Layout 自体は常に表示される
* 切り替わるのは <Outlet /> の中だけ
* これが、React Router の「共通レイアウト＋ページ切り替え」の仕組みです。

### 7.3 Home.tsxコンポーネント（トップページ）の作成

src/App.tsx で、react-router-domを使う場合は、

```App.tsx
<Route index element={<Home />} />
```

のように指定します。

```bash
mkdir -p src/pages && touch src/pages/Home.tsx
```

shadcnのCard コンポーネントを追加する

```bash
npx shadcn@latest add card
```

Home.tsx コンポーネントの作成

```Home.tsx
import { Plus } from "lucide-react";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";

const Home = () => {
  return (
    <div>
      <Card className="m-auto w-1/2 border-0 shadow-none">
        <CardHeader className="px-4 pb-3">
          <CardTitle className="text-lg font-medium">
            新しいノートを作成してみましょう
          </CardTitle>
        </CardHeader>
        <CardContent className="px-4">
          <div className="flex gap-2">
            <input
              className="block h-9 w-full flex-1 appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-slate-500 focus:ring-slate-500 focus:outline-none sm:text-sm"
              placeholder="ノートのタイトルを入力"
              type="text"
            />
            <button className="flex justify-center rounded-md border border-transparent bg-slate-900 px-4 py-2 text-sm font-medium text-white shadow-sm focus:ring-2 focus:ring-slate-500 focus:ring-offset-2 focus:outline-none disabled:cursor-not-allowed disabled:opacity-50">
              <Plus className="h-4 w-4" />
              <span className="ml-1">ノート作成</span>
            </button>
          </div>
        </CardContent>
      </Card>
    </div>
  );
};

export default Home;

```

解説

pagesディレクトリは、ルーティング単位の画面コンポーネントを置く場所です。
Home.tsx はトップページに対応する画面になります。

### 7.4 pages/notes/NoteDetail.tsx コンポーネント(詳細ページ)を作成する

```bash
mkdir -p src/pages/notes && touch src/pages/notes/NoteDetail.tsx
```

```pages/notes/NoteDetail.tsx
import { useParams } from "react-router-dom";

const NoteDetail = () => {
  const { id } = useParams();
  return <div>ノートID: {id}</div>;
};

export default NoteDetail;

```

解説

NoteDetail コンポーネントは、
URL に含まれている「ノートID」を取得して表示するページです。

```URL
/notes/123
```

のような URL にアクセスすると、
123 の部分を React Router から受け取ることができます。

useParams とは？
import { useParams } from "react-router-dom";

useParams は、
URL のパラメータ（:id など）を取得するための React Router のフックです。

URL とコードの対応関係

例えば、ルーティングを次のように定義している場合：

src/App.tsx
```
<Route path="/notes/:id" element={<NoteDetail />} />
```

:id の部分が「URL パラメータ」になります。

ノートIDを取得する
const { id } = useParams();

useParams() は URL パラメータをオブジェクトとして返す

今回は :id という名前なので、id を取り出している

取得した ID を表示する
return <div>ノートID: {id}</div>;

ここでは確認のために、
取得したノートIDをそのまま画面に表示しています。

※ 実際のアプリでは、この id を使って
ノートのデータを取得することになります。

### 7.5 pages/Signin コンポーネント(サインイン)を作成します。

```bash
touch src/pages/Signin.tsx
```

```pages/Signin.tsx
import { Link } from "react-router-dom";

const Signin = () => {
  return (
    <div className="min-h-screen bg-gray-100 px-4 py-10 sm:px-6 lg:px-8">
      <div className="flex flex-col items-center">
        <h2 className="text-3xl font-extrabold text-gray-900">
          Notionクローン
        </h2>

        <div className="mt-8 w-full max-w-md">
          <div className="bg-white px-4 py-8 shadow sm:rounded-lg sm:px-10">
            <div className="space-y-6">
              {/* メールアドレス */}
              <div>
                <label
                  htmlFor="email"
                  className="block text-sm font-medium text-gray-700"
                >
                  メールアドレス
                </label>
                <div className="mt-1">
                  <input
                    id="email"
                    name="email"
                    type="email"
                    placeholder="メールアドレス"
                    className="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-slate-500 focus:ring-slate-500 focus:outline-none sm:text-sm"
                  />
                </div>
              </div>

              {/* パスワード */}
              <div>
                <label
                  htmlFor="password"
                  className="block text-sm font-medium text-gray-700"
                >
                  パスワード
                </label>
                <div className="mt-1">
                  <input
                    id="password"
                    name="password"
                    type="password"
                    placeholder="パスワード"
                    className="block w-full appearance-none rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm focus:border-slate-500 focus:ring-slate-500 focus:outline-none sm:text-sm"
                  />
                </div>
              </div>

              {/* ログインボタン */}
              <div>
                <button
                  type="button"
                  className="flex w-full justify-center rounded-md border border-transparent bg-slate-900 px-4 py-2 text-sm font-medium text-white shadow-sm focus:ring-2 focus:ring-slate-500 focus:ring-offset-2 focus:outline-none disabled:cursor-not-allowed disabled:opacity-50"
                >
                  ログイン
                </button>
              </div>

              {/* サインアップへのリンク */}
              <div className="mt-4 text-center text-sm">
                登録は
                <Link to="/signup" className="ml-1 underline">
                  こちら
                </Link>
                から
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

export default Signin;

```

解説

このコンポーネントの役割

Signin コンポーネントは、
ユーザーがメールアドレスとパスワードを入力してログインする画面を表示するためのものです。

import 文の解説
```ts
import { Link } from "react-router-dom";
```

Link は 画面遷移用のコンポーネントです

<a> タグの代わりに使うことで、

ページのリロードをせず

React Router のルーティングを使った遷移ができます

今回は「サインアップ画面へ移動するリンク」に使っています。

http://localhost:5173/signin

### 7.6 pages/Signup.tsx コンポーネント（サインアップ）を作成

```bash
touch src/pages/Signup.tsx
```

```src/pages/Signup.tsx
const Signup = () => {
  return (
    <div className="min-h-screen bg-gray-100 px-4 py-10 sm:px-6 lg:px-8">
      <div className="flex flex-col items-center">
        <h2 className="text-3xl font-extrabold text-gray-900">
          Notionクローン
        </h2>

        <div className="mt-8 w-full max-w-md">
          <div className="bg-white px-4 py-8 shadow sm:rounded-lg sm:px-10">
            <form className="space-y-6" noValidate>
              {/* ユーザー名 */}
              <div>
                <label
                  htmlFor="username"
                  className="block text-sm font-medium text-gray-700"
                >
                  ユーザー名
                </label>
                <input
                  id="username"
                  name="username"
                  type="text"
                  autoComplete="username"
                  className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2 shadow-sm focus:border-slate-500 focus:ring-slate-500 sm:text-sm"
                />
              </div>

              {/* メールアドレス */}
              <div>
                <label
                  htmlFor="email"
                  className="block text-sm font-medium text-gray-700"
                >
                  メールアドレス
                </label>
                <input
                  id="email"
                  name="email"
                  type="email"
                  autoComplete="email"
                  className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2 shadow-sm focus:border-slate-500 focus:ring-slate-500 sm:text-sm"
                />
              </div>

              {/* パスワード */}
              <div>
                <label
                  htmlFor="password"
                  className="block text-sm font-medium text-gray-700"
                >
                  パスワード
                </label>
                <input
                  id="password"
                  name="password"
                  type="password"
                  autoComplete="new-password"
                  className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2 shadow-sm focus:border-slate-500 focus:ring-slate-500 sm:text-sm"
                />
              </div>

              <button
                type="submit"
                className="w-full rounded-md bg-slate-900 px-4 py-2 text-sm font-medium text-white shadow-sm focus:ring-2 focus:ring-slate-500 focus:ring-offset-2"
              >
                登録
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  );
};

export default Signup;
```

解説

このコンポーネントの役割

Signup コンポーネントは、
新規ユーザー登録（サインアップ）画面を表示するためのものです。

http://localhost:5173/signup

### 7.7 動作確認

```bash
npm run dev
```

トップページ

http://localhost:5173/ 

詳細ページ

http://localhost:5173/notes/1

サインアップ

http://localhost:5173/signup

サインイン

http://localhost:5173/signin

## 8. 認証機能（ユーザー登録機能）を作成

### 8.1 Supabaseの解説とセットアップ

Supabase は、認証・データベース・ストレージなどをまとめて提供するBaaS(Backend as a Service)です。
本章では、Supabaseを利用してユーザー登録機能の準備を行います。

まだ Supabaseのアカウントを所持していない場合は、以下の公式サイトからアカウントを作成してください。

[https://supabase.com/](https://supabase.com/)

### 8.2 プロジェクトを作成

アカウント作成後、Supabaseのダッシュボードにアクセスします。

[https://supabase.com/dashboard](https://supabase.com/dashboard)

1. + New project ボタンをクリックします

2. Create a new project 画面で、以下の項目を設定します。
   * Organization
     → 初期設定の状態で大丈夫です。
   * Project name
     → `notion-clone-app`
   * Database password
     → 忘れないパスワードを入れてください。
   * Region
     →　`Northeast Asia (Tokyo)`　を選択します。

3. Create new project ボタンをクリックします。

### 8.3 ライブラリをインストール

ReactからSupabaseを利用するため、公式SDKをインストールします。

```bash
npm install @supabase/supabase-js
```

### 8.4 SupaBase の Auth Providerの設定に（Email認証）

次に、ユーザー登録で使用する Email 認証を有効化します。

1. dashboard 左側メニューから「Authentication」を選択します。

2. Authentication 配下の「Sign In / Providers」お選択します。

3. Sign In / Providersの一覧から、ページをスクロールして「Auth Providers」の一覧から「Email」を選択します。

4. Email の設定画面が表示されるので、×をクリックします。 

5. Auth Providers のEmail 右横にあるステータスが「Disabled」から「Enabled」になっていることを確認します。

### 8.5 Supabeseクライアント初期化ファイルを作成

ReactアプリからSupabaseに接続するための初期化ファイルを作成します。

このファイルはアプリ全体で共通して利用するSupabaseクライアントを定義する役割を持ちます。

以下のコマンドを実行し、src/lib/supabase.tsファイルを作成します。

```bash
mkdir -p src/lib/ && touch src/lib/supabase.ts
```

src/lib/supabase.ts（サンプル）

※以下はサンプルです。実際の値は後ほど環境変数から読み込みます。

```ts
import { createClient } from '@supabase/supabase-js'

// Create a single supabase client for interacting with your database
 export const supabase = createClient('https://XXXXXXXXXX.supabase.co', 'publishable-or-anon-key')

```

解説

`createClient`関数は、以下の2つの引数を指定することでSupabaseクライアントを初期化します。

* プロジェクトURL
* APIキー

このクライアントを通じて、認証・データ取得・データ操作などの機能などの機能が利用できるようになります。

### 8.6 環境変数（.env）を作成する

APIキーやURLを直接コードに書かず、環境変数として管理します。
これにより、開発環境と本番環境を分けて管理できます。

* .envファイルを作成

プロジェクト直下に、`.env` ファイルを作成します。

```bash
touch .env
```

本プロジェクトは、Viteを使用しているため、環境変数名は、`VITE_`から始める必要があります。

```env
VITE_SUPABASE_URL = ""
VITE_SUPABASE_API_KEY = ""
```

* Viteでは、`VITE_`プレフィックスが付いた環境変数のみが`import.meta.env`から参照可能です。

* Supabase の URL/APIキーを取得する

1. dashboard 左側にあるメニューから、「Project Overview」をクリックします。

2. Connect to your project セクションにあるProject APIを確認します。

以下の情報を使用します。

* Project URL
  `https://XXXXXXXXXXXXXXXXXXXX.supabase.co`

* anon public key（Publishable API Key）
  `sb_publishable_XXXXXXXXX_XXXXXXXXXXXX_XXX-XXXX`

* フロントエンド（React）では必ずanon public key（Publishable）を使用してください。

* `service_role key` はサーバー専用のため、絶対に使用しません。

取得した値を、先に作成した`.env`ファイルに記述します。

```env
VITE_SUPABASE_URL = "https://XXXXXXXXXXXXXXXXXXXX.supabase.co"
VITE_SUPABASE_API_KEY = "sb_publishable_XXXXXXXXX_XXXXXXXXXXXX_XXX-XXXX"
```

### 8.7 .gitignoreに.envを追加

APIキーをGitHubに公開しないよう、`.gitignore`ファイルに`.env`を追加します。

```txt
.env
```

### 8.8 `supabase.ts`を環境変数対応に修正

最後に、`supabase.ts`を環境変数から値を読み込むように修正します。

```ts
import { createClient } from "@supabase/supabase-js";

// Create a single supabase client for interacting with your database
export const supabase = createClient(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_API_KEY,
);

```

※ 注意点

* `.env` を変更した場合は開発サーバーを再起動しないと反映されません
* anon public key は公開されても問題ありませんが、`service_role key`は絶対にフロントエンドに記載しないでください。

次の章では、Supabaseを利用してユーザー登録（signUp）処理を実装していきます。

### 8.9 Supabase Authを使ったユーザー登録機能の実装

アプリを作る上で、主なビジネスロジック（アプリの処理の中身）は、大きく次の2つに分けられます。

* 認証まわりの機能
* ノートまわりの機能

これらのロジックは、機能ごとに管理しやすくするため、`modules`フォルダにまとめて格納します。

今回は、ユーザー登録（サインアップ）を担当する認証機能のモジュールを作成します。
まずは、`src/modules/auth/auth.repository.ts`ファイルを作成します。

```bash
mkdir -p src/modules/auth && touch src/modules/auth/auth.repository.ts
```

このコマンドでは、次のものを作成しています。

* `auth`: 認証機能用のフォルダ
* `auth.repository.ts`: 認証処理をまとめるファイル

#### 8.9.1 auth.repository.ts の役割

`auth.repository.ts` は、Supabase の認証 API を呼び出す処理をまとめたファイルです。

Reactコンポーネントから直接 Supabase を操作せず、この`auth.repository.ts` を経由して認証処理を行うことで、

* Supabaseと通信する処理
* 画面（Reactコンポーネント）

を分離できます。

これにより、コードの見通しがよくなり、保守・テストもしやすくなります。

#### 8.9.2 `src/modules/auth/auth.repository.ts` なのか？

このファイル名と配置は、「何をするコードか」「どこに属するか」が一目で分かるように設計されています。

* `modules`フォルダを使う理由

このアプリでは、主に次のような機能単位でロジックを管理します。

* 認証（auth）
* ノート（notes）

UI（components）やページ（pages / app）とは分離し、「アプリの振る舞いを決めるロジック」を置く場所として`modules`を使っています。

* `auth`フォルダなのか

`auth` フォルダは、認証に関する処理をまとめるためのフォルダです。
ログイン・ログアウト・ユーザー登録など、認証に関するロジックはすべてここに集約します。

* `auth.repository.ts`というファイル名なのか？

Repository（リポジトリ）とは、外部のサービスと通信する処理をまとめた層のことです。

今回の場合は、Supabase の 認証系APIとのやり取りを、この Repository に集約しています。

このようにすることで、

* Supabase の仕様が変わっても影響範囲を限定できる
* 画面側のコードをシンプルに保てる

といったメリットがあります。

※ このような構成は、Repository パターンと呼ばれる設計の考え方の一部です。

#### 8.9.3 auth.repository.ts の実装

`auth.repository.ts` の実装

```ts
import { supabase } from "@/lib/supabase";

export const authRepository = {
  async signup(name: string, email: string, password: string) {
    const { data, error } = await supabase.auth.signUp({
      email,
      password,
      options: {
        data: {
          name,
        },
      },
    });

    return { data, error };
  },
};
```

解説

このコードは、Supabaseを使ってユーザー登録（サインアップ）を行う処理をまとめたものです。
画面（Reactコンポーネント）から直接Supabaseを操作せず、`authRepository.signup`を通じてユーザー登録を行います。

このように repository を用意することで、

* Supabase と通信する処理
* 画面表示やユーザー操作を扱う処理

を分離できます。

「責務を分ける構成」を理解することを重視しています。

次の章では、この `authRepository.signup` を React コンポーネントから呼び出し、
実際にユーザー登録画面を実装していきます。

#### 8.9.2 Supabase Auth で作る安全なサインアップフォーム

Step 1: 必要なモジュールのimport

目的

* React の状態管理フック `useState` を使えるようにする
* Supabase への登録処理をまとめた `authRepository` を使えるようにする

```Signup.tsx
// src/pages/Signup.tsx
import { authRepository } from "@/modules/auth/auth.repository";
import { useState } from "react";
```

解説

* React のフックはフォームの入力値やエラー表示の状態管理に必要
* `authrepository`はSupabaseのSignup処理をラップしており、エラーハンドリングを簡単にできるラッパー

Step 2: フォームの骨組み作成（見た目だけ）

目的

* フォームの見た目を先に確認できる
* 名前・メール・パスワード入力欄、ボタン、メッセージ表示の枠だけ作る

```Signup.tsx
const Signup = () => {
  // フォームの状態管理
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  // エラー・成功メッセージ
  const [errors, setErrors] = useState<string[]>([]);
  const [success, setSuccess] = useState<string | null>(null);

  // 登録中状態
  const [loading, setLoading] = useState(false);

  // ※ handleSignup は Step 3 で定義

```

return 内の骨組み

```src/app/Signup.tsx
  // ※ handleSignup は Step 3 で定義
  return (
    <div className="min-h-screen bg-gray-100 px-4 py-10 sm:px-6 lg:px-8">
      <div className="flex flex-col items-center">
        <h2 className="text-3xl font-extrabold text-gray-900">
          Notionクローン
        </h2>

        <div className="mt-8 w-full max-w-md">
          <div className="bg-white px-4 py-8 shadow sm:rounded-lg sm:px-10">
            <form className="space-y-6" onSubmit={(e) => e.preventDefault()} noValidate>
              {/* エラー・成功メッセージ枠 */}
              {errors.length > 0 && (
                <div
                  role="alert"
                  className="my-8 border-l-10 border-[#ffc06e] bg-yellow-50 px-4 py-2 text-gray-800"
                >
                  <ul className="list-disc pl-5">
                    {errors.map((error, index) => (
                      <li key={index}>{error}</li>
                    ))}
                  </ul>
                </div>
              )}
              {success && (
                <div
                  role="status"
                  className="my-8 border-l-10 border-green-500 bg-green-50 px-4 py-2 text-gray-800"
                >
                  <p className="m-0 p-0">{success}</p>
                </div>
              )}

              {/* 入力欄 */}
              <div>
                <label className="block text-sm font-medium text-gray-700">
                  ユーザー名
                </label>
                <input
                  type="text"
                  value={name}
                  onChange={(e) => setName(e.target.value)}
                  className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2 shadow-sm"
                />
              </div>

              <div>
                <label className="block text-sm font-medium text-gray-700">
                  メールアドレス
                </label>
                <input
                  type="email"
                  value={email}
                  onChange={(e) => setEmail(e.target.value)}
                  className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2 shadow-sm"
                />
              </div>

              <div>
                <label className="block text-sm font-medium text-gray-700">
                  パスワード
                </label>
                <input
                  type="password"
                  value={password}
                  onChange={(e) => setPassword(e.target.value)}
                  className="mt-1 block w-full rounded-md border border-gray-300 px-3 py-2 shadow-sm"
                />
              </div>

              <button
                type="submit"
                disabled={loading}
                className="w-full rounded-md bg-slate-900 px-4 py-2 text-sm font-medium text-white disabled:opacity-50"
              >
                {loading ? "登録中..." : "登録"}
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  );
};

export default Signup;
```

解説

* この段階ではフォームの見た目だけ確認可能。
* 入力フォームには、`name・email・password`の3っの入力欄があります。
* 入力欄やボタン、エラー・成功表示の枠を先に作る

動作確認

```bash
npm run dev  
```

ブラウザーで[http://localhost:5173/signup](http://localhost:5173/signup)を開き、フォームの見た目を確認します。

Step 3: 登録処理の追加（handleSignup）

目的

* ボタンを押したら Supabase にアカウントを作れるようにする

* 成功／失敗のメッセージ表示

```src/pages/Signup.tsx
const handleSignup = async (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();

  setErrors([]);
  setSuccess(null);

  try {
    // Supabase 登録処理
    const { error } = await authRepository.signup(name, email, password);

    if (error) {
      setErrors([error.message]);
      return;
    }

    setSuccess("サインアップが成功しました。");
  } catch (err) {
    setErrors([err instanceof Error ? err.message : "サインアップ中にエラーが発生しました"]);
  }
};
```

フォームの onSubmit を handleSignup に置き換え

```src/pages/Signup.tsx
return (
  <div className="min-h-screen bg-gray-100 px-4 py-10 sm:px-6 lg:px-8">
    <div className="flex flex-col items-center">
      <h2 className="text-3xl font-extrabold text-gray-900">
        Notionクローン
      </h2>

      <div className="mt-8 w-full max-w-md">
        <div className="bg-white px-4 py-8 shadow sm:rounded-lg sm:px-10">
          <form className="space-y-6" onSubmit={handleSignup} noValidate>

            {/* 省略 */}

```

動作確認

1. 開発サーバー起動

```bash
npm run dev  
```

2. ブラウザーで[http://localhost:5173/signup](http://localhost:5173/signup)を開く。

3. テスト用の入力値：

* ユーザー名 → testuser1
* メールアドレス → t1@t.co
* パスワード → 123456

4. 登録ボタンをクリック

5. Supabaseダッシュボードで確認：

* ブラウザーより、[https://supabase.com/dashboard/](https://supabase.com/dashboard/) にアクセス
* `notion-clone-app` を選択
* 左側メニューのAuthentication → Users で登録済みアカウントを確認の一覧が表示されます。











この時点ではローディングバリデーションや日本語化は未実装




Step 5: 






1. フォームのステートを作成する

各項目に、入力された情報を保持するためのステートを作成します。

```src/pages/Signup.tsx
// src/pages/Signup.tsx
import { useState } from "react";

const Signup = () => {
  // フォームの状態管理
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  return (
    <div class 
```

解説

```tsx
const [name, setName] = useState("");
```

このコードは、画面の状態（ステート）を1つ作っています。
フォームに入力された文字など、画面と連動して変わる値を管理するために使います。

`name`と`setName`の役割

* `name` は今の状態の値（現在入力されている内容）
* `setName`は状態を更新するための関数

constは再代入ができないと学びました。

```ts
const name = "太朗";
name = "花子"
```
はできない。

代わりに、Reactの`setName`関数を使って、状態の更新を依頼します。

```ts
setName('花子');
```

setName関数を呼び出すと何が起きるのか

`setName`関数を呼び出すと、

1. Reactが内部の状態を更新する。
2. コンポーネントを最初から再実行する
3. 次の描画で`name`の値として"花子"が渡される

これは、 `const name`を上書きしている訳ではありません。

新しい状態で再描画されています。

値とステートの違い

* 値：ただのデータ
* ステート：画面と連動して管理され、更新されると再描画される値

2. inputタグとステートを連携させる

`input`タグの`onChange`イベントを使って、入力された値がステートに反映されるようにします。

* ユーザー名

```src/pages/Signup.tsx
                <label
                  htmlFor="username"
                  className="block text-sm font-medium text-gray-700"
                >
                  ユーザー名
                </label>
                <input
                  onChange={(e) => setName(e.target.value)}
```

* メールアドレス

```src/pages/Signup.tsx
                <label
                  htmlFor="email"
                  className="block text-sm font-medium text-gray-700"
                >
                  メールアドレス
                </label>
                <input
                  onChange={(e) => setEmail(e.target.value)}
```

* パスワード

```src/pages/Signup.tsx
                <label
                  htmlFor="password"
                  className="block text-sm font-medium text-gray-700"
                >
                  パスワード
                </label>
                <input
                  onChange={(e) => setPassword(e.target.value)}
                  id="password"
```

2. サインアップ処理を実装する

作成したステートを使って、`authRepository.signup`を呼び出します。

```src/pages/Signup.tsx
import { authRepository } from "@/modules/auth/auth.repository";
import { useState } from "react";

const Signup = () => {
  // フォームの状態管理
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  
  // サインアップ処理
  const signup = async (e: React.FormEvent) => {
    e.preventDefault();
    const user = await authRepository.signup(name, email, password);
    console.log(user);
  };

  return (


```

解説

① `const signup = async (...) => {}`

* `signup`はサインアップ（新規登録）を行う関数です。
* `async`をつけることで時間がかかる処理（通信）を待てる関数になります。
* `e`はフォーム送信じのイベント情報です。
* ` React.FormEvent`は、「これはフォームイベントです」とTypeScriptに伝えています。
* `e.preventDefault();`
* フォーム送信時のページリロードを止める命令です。

3. バリデーションを追加する

いずれかの項目が未入寮の場合、登録ボタンをクリっくできないようにします。

```src/pages/Signup.tsx
              <button
                disabled={!name || !email || !password}
                type="submit"
                className="w-full rounded-md bg-slate-900 px-4 py-2 text-sm font-medium text-white shadow-sm focus:ring-2 focus:ring-slate-500 focus:ring-offset-2 disabled:opacity-50"
              >
                登録
              </button>
```

さらに、Supabase Authの入力条件があるのでバリデーションを追加します。

* email の項目に、正しいメール形式である
* password には最低 6文字である

ことが必要になります。

```
const Signup = () => {
  // フォームの状態管理
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  // サインアップ処理
  const signup = async (e: React.FormEvent) => {
    e.preventDefault();
    console.log("入力された値:", JSON.stringify({ name, email, password }));

    //email バリデーション
    const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

if (!emailPattern.test(email)) {
  alert("メールアドレスの形式が正しくありません");
  return;
}

    // パスワード バリデーション
    if(password.length < 6) {
      alert("パスワードは6文字以上で入力してください。");
      return;
    }
    // サインアップ処理の呼び出し
    const user = await authRepository.signup(name, email, password);
    console.log(user);
  };

  return (

```







4. 登録ボタンでサインアップ処理を呼び出す

formタグに`onSubmit={signup}`を追加します。

```src/pages/Signup.tsx
<form className="space-y-6" onSubmit={signup} noValidate>
```

解説

`onSubmit={signup}`を指定することで、フォームが送信されたタイミングで`signup`関数が実行されます。

### 9.11 動作確認

開発サーバーを起動します。

```bash
npm run dev
```

次のURLをブラウザで開いてください。

[http://localhost:5173/signup](http://localhost:5173/signup) 

Google Chromeの上部のメニューから
表示　→　開発／管理　→　デベロッパーツール　をクリックします。

デベロッパーツールが表示されたら、Consoleタブを開きます。

ユーザー名・メールアドレス・パスワードいずれかが未入力の場合、登録ボタンは無効になり、処理は実行されません。

| 項番 | ユーザー名 | メールアドレス | パスワード | 登録ボタン | console.logの結果 |
| --  | -------- | ----------- | --------- | -------- | ---------------- |
| ①  | 未記入    | 未記入       | 未記入     | 押せない  | 表示されない        |
| ②  | 記入     | 未記入       | 未記入     | 押せない  | 表示されない        |
| ③  | 未記入    | 記入        | 未記入     | 押せない  | 表示されない        |
| ④  | 未記入   | 未記入       | 記入       | 押せない  | 表示されない         |
| ⑤  | 記入     | 記入        | 未記入     | 押せない  | 表示されない         |
| ⑥  | 記入     | 未記入       | 記入      | 押せない  | 表示されない         |
| ⑦  | 未記入   | 記入        | 記入       | 押せない  | 表示されない         |
| ⑧  | 記入   | 記入        | 記入       | 押せる  | 表示される         |

最後に、全ての項目が記入されたら、登録ボタンの背景色が黒くなり、クリックできてconsole.logの結果が表示されることを確認してください。

正しい値が入力された場合において、登録できたらSupabaseより登録できたか確認します。

## Supabaseのダッシュボードよりアカウントが登録されたか確認します

ブラウザーより、[https://supabase.com/dashboard/](https://supabase.com/dashboard/) を表示して、

notion-clone-app をクリックします。

Supabaseのダッシュボードが表示されます。

左側から、Authenticationをクリックします。

Users の一覧が表示されます。









