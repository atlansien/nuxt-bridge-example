# nuxt-bridge-example

`yarn create nuxt-app`で作った素のアプリから`nuxt-bridge`が使えるようにした。

手順は公式に書いてある通りに行った
https://v3.nuxtjs.org/bridge/overview

## Build Setup

```bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev

# build for production and launch server
$ yarn build
$ yarn start

# generate static project
$ yarn generate
```

## nuxt-bridge化手順

### まずはアプリを作る
```bash
yarn create nuxt-app
```
選択技では`TypeScript`とテストフレームワークに`Jest`を選択。

### package.jsonからnuxt2の記述を消し、nuxt-edgeに置き換えてインストール

```json
- "nuxt": "^2.15.0"
+ "nuxt-edge": "latest"
```

What's nuxt-edge?
https://www.npmjs.com/package/nuxt-edge

```bash
yarn install
```

### nuxt-bridgeと依存関係をインストール

```bash
yarn add --dev @nuxt/bridge@npm:@nuxt/bridge-edge
```

### package.jsonのscriptsを書き換え

Nuxt3 では`nuxi`というコマンドが導入されたようなので、nuxi に準じて Script の更新

```json
  "scripts": {
-   "dev": "nuxt",
+   "dev": "nuxi dev",
-   "build": "nuxt build",
+   "build": "nuxi build",
-   "start": "nuxt start",
+   "start": "nuxi preview"
  }
```

今回は設定していないが、nuxt.config.js で`target: static`を設定している場合は generate コマンドも nuxi に変える必要あり

```json
    "build": "nuxi generate"
```

### nuxt.config.jsの記述方法変更

create で生成されるnuxt.config.jsに使われている`require`構文などは非推奨のため、静的 import を使用する。

```js
import { defineNuxtConfig } from '@nuxt/bridge'

export default defineNuxtConfig({
  // Your existing configuration
})
```
あとは拡張子を変更、`ts`にした

### tsconfig.jsonにNuxtの自動生成された型を使用できるようにする

```json
{
+ "extends": "./.nuxt/tsconfig.json",
  "compilerOptions": {
    ...
  }
}
```

### 不要な依存関係を remove する
nuxt-bridgeでサポートされている機能と重複する依存関係を remove する。今回は `@nuxt/typescript-build`のみだった

### .outputディレクトリを生成し、.gitignoreに追加。

### Tutorial.vueを削除し、index.vueを更新。
終わり🎉