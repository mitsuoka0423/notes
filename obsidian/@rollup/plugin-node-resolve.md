## 概要
- [[node_modules]] でサードパーティモジュールを使うために、 Node resolution アルゴリズムを使ってモジュールを探す [[Rollup]] プラグイン

## 詳細
### `resolve`

### `nodeResolve`
`rollup.config.mjs`
```js
import { nodeResolve } from '@rollup/plugin-node-resolve';

export default {
  input: 'index.js',
  output: {
    dir: 'output',
    format: 'cjs'
  },
  plugins: [nodeResolve()]
};
```
`index.js`
```js
const HTMLparse = require('node-html-parser');

console.log(HTMLparse);
```
`output/index.js`
```js
'use strict';

const HTMLparse = require('node-html-parser');

console.log(HTMLparse);
```
→役割がよくわからない

#### [[@rollup/plugin-commonjs]] と合わせて使う
`rollup.config.mjs`
```js
import { nodeResolve } from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';

export default {
  input: 'index.js',
  output: {
    dir: 'output',
    format: 'iife',
    name: 'MyModule'
  },
  plugins: [nodeResolve(), commonjs()]
};
```
`index.js`
```js
const HTMLparse = require('node-html-parser');

console.log(HTMLparse);
```
`output/index.js`
```js
var MyModule = (function () {
	'use strict';

	var commonjsGlobal = typeof globalThis !== 'undefined' ? globalThis : typeof window !== 'undefined' ? window : typeof global !== 'undefined' ? global : typeof self !== 'undefined' ? self : {};

// 略

})();

```
→ `require` がなくなった
## 公式ドキュメント
- https://github.com/rollup/plugins/tree/master/packages/node-resolve

## 関連リンク

