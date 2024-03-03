## 概要
- [[Rollup]]  と [[Babel]] をシームレスに結合するための [[Rollup]] プラグイン

## 詳細
### サンプルコード
`rollup.config.mjs`
```js
import { babel } from '@rollup/plugin-babel';

export default {
  input: 'index.js',
  output: {
    dir: 'output',
    format: 'es'
  },
  plugins: [babel({ babelHelpers: 'bundled' })]
};
```
`index.js`
```js
const HTMLparse = require('node-html-parser');

console.log(HTMLparse);
```
`output/index.js`
```js
const HTMLparse = require('node-html-parser');
console.log(HTMLparse);
```

## 公式ドキュメント


## 関連リンク

