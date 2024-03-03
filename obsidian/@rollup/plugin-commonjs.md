## 概要
- [[CommonJS]] モジュールを [[ESMAScript]] に変換する [[Rollup]] プラグイン

## 詳細
- [[@rollup/plugin-node-resolve]] の `v13.0.6` 以上が必要
- `format` に [[IIFE]] を指定することで、[[require]] でロードしているモジュールが即時実行関数式に変換される

### サンプルコード
```js
// rollup.config.js
import resolve from '@rollup/plugin-node-resolve';
import commonjs from '@rollup/plugin-commonjs';

export default {
  input: 'main.js',
  output: {
    file: 'bundle.js',
    format: 'iife',
    name: 'MyModule'
  },
  plugins: [commonjs(), resolve()]
};
```

### 試してみた
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
- https://github.com/rollup/plugins/tree/master/packages/commonjs

## 関連リンク

