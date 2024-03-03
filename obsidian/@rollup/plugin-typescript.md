## 概要
- [[Rollup]] と [[TypeScript]] をシームレスに結合するための [[Rollup]] プラグイン

## 詳細
- [[TypeScript]] 3.7 以上が必要
- [[tslib]] も必要

### 試してみた
`rollup.config.mjs`
```js
import typescript from '@rollup/plugin-typescript';

export default {
  input: 'index.ts',
  output: {
    dir: 'output',
    format: 'es'
  },
  plugins: [
    typescript(),
  ]
};

```
`index.ts`
```ts
import { hello } from "./module";

console.log(hello);

const a = (input: number): string => {
  return String(input);
}

console.log(a);
```
`output/index.js`
```js
import { hello } from "./module";

console.log(hello);

const a = (input: number): string => {
  return String(input);
}

console.log(a);
```
### `require` を使ってるパターン
`rollup.config.mjs`
```js
import typescript from '@rollup/plugin-typescript';

export default {
  input: 'index.ts',
  output: {
    dir: 'output',
    format: 'es'
  },
  plugins: [
    typescript(),
  ]
};

```
`index.ts`
```ts
const HTMLParser = require('node-html-parser');

import { hello } from "./module";
console.log(hello);

const a = (input: number): string => {
  return String(input);
}
console.log(a);
```
`output/index.js`
```js
Object.defineProperty(exports, "__esModule", { value: true });
require('node-html-parser');
const module_1 = require("./module");
console.log(module_1.hello);
const a = (input) => {
    return String(input);
};
console.log(a);
```
→ `require` が残る（そりゃそう）

## 公式ドキュメント
- https://www.npmjs.com/package/@rollup/plugin-typescript

## 関連リンク

