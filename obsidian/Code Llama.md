## 概要
- Meta が開発しているプログラミング言語処理に特化した自然言語処理モデルの一つ

## 詳細
![image](https://i.imgur.com/OQ1daGo.png)

- Introducing Code Llama, a [[state-of-the-art]] large language model for coding
- [[Code Llama]] は、Llama 2をベースとしたコード用の大規模言語モデルファミリで、オープンモデルの中でも
	- 最先端の性能
	- インフィリング機能
	- 大規模な入力コンテキストのサポート
	- プログラミングタスクのゼロショット命令追従機能
	を提供します。
	- 基礎モデル（Code Llama）
	- Python特化モデル（Code Llama - Python）
	- 命令追従モデル（Code Llama - Instruct）
- があり、それぞれ
	- 7B
	- 13B
	- 34B
	のパラメータを持ちます。
- 全てのモデルは16kトークンのシーケンスで学習され、最大100kトークンの入力で改善を示す。
- 7Bと13BのCode LlamaとCode Llama - Instructのバリエーションは、周囲のコンテンツに基づく穴埋めをサポートしています。
- Code LlamaはLlama 2をより高いサンプリングコードを使って微調整することで開発された。
- Llama 2と同様に、微調整されたバージョンのモデルにもかなりの安全性緩和を適用しました。
- モデルのトレーニング、アーキテクチャとパラメータ、評価、責任あるAIと安全性の詳細については、研究論文を参照してください。
- Code Llamaを含むLlamaマテリアルのコード生成機能によって生成された出力は、オープンソースライセンスを含むサードパーティライセンスの対象となる場合があります。

## 公式ドキュメント
- https://github.com/facebookresearch/codellama
- https://ai.meta.com/blog/code-llama-large-language-model-coding/

## 関連リンク

