#Nix 
# Nix language
- ## 特徴
	- creating and composing _derivations_
		- derivative 導関数, 微分
		- derivation 導出物
	- domain-specific 
		  -  ソフトウェア開発
		  - システム管理
	- purely functional
		  - 副作用のない関数を使用
			  - 副作用がある
				- 関数がグローバル変数や関数外部の変数の値を変更する。
				- ファイルを作成、変更、削除するなど、ファイルシステムに対する操作を行う。
				- データベースにデータを追加、更新、削除するなどの操作を行う。
				- インターネットを通じて外部のサーバーとデータをやり取りする。
				- ユーザーからの入力を受け取ったり、画面に出力を行ったりする。
				- 関数が呼び出された際に、システムやオブジェクトの状態を変更する。
	- lazily evaluated
		- 遅延評価
			- 必要な計算のみを行う
			- 式がその値が必要になるまで評価されない
	- dynamically typed programming language.
		- 動的型付け
			- 実行時に型チェック
	- creating and composing _derivations_
- ## Nix Language のコンセプトと構造
	- Assigning names and accessing values _名前の割当と値のアクセス_
	- Declaring and calling functions _関数の宣言と呼び出し_
	- Built-in and library functions _組み込み関数とライブラリ関数_
	- Impurities to obtain build inputs _ビルド入力を取得するための非純粋性_
	- Derivations that describe build tasks _ビルドタスクを記述する導出物_
- ## Language _言語_
	- syntax _構文_
	- semantics _意味論_
- ## Libraries _ライブラリ_
	- `builtins`
	- `import`
	- `pkgs.lib`
- ## Developer tools _開発者ツール_
- ## Generic build mechanisms _汎用ビルドメカニズム_
- ## Composition and configuration mechanisms _構成と設定メカニズム_
- ## Ecosystem-specific packaging mechanisms _エコシステム固有のパッケージングメカニズム_
- ## NixOS module system _NixOSモジュールシステム_

- Interactive evaluation
	```bash
	nix repl 
	Welcome to Nix 2.18.2. Type :? for help.
	nix-repl> 1  + 2
	3
	```
	- lazy evaluation
		-  入力と出力が合致しない場合がある。
			```bash
			nix-repl> {a.b.c = 1;}
			{ a = { ... }; }
			```
		- `:p`
			```bash
			nix-repl> :p {a.b.c = 1;}
			{ a = { b = { c = 1; }; }; }
			```
- Evaluating Nix files
	- Nixファイルの評価する
		```bash
		$ echo 1 + 2 > file.nix
		$ nix-instantiate --eval file.nix
		3
		
		$ cat file.nix
		1 + 2
		```
	- lazy evaluation `--strict`
		```bash
		$ echo "{ a.b.c = 1; }" > file.nix
		$ nix-instantiate --eval file.nix
		{ a = <CODE>; }
		
		$ nix-instantiate --eval --strict file.nix
		{ a = { b = { c = 1; }; }; }
		```
- Notes on white space
	以下は同じである
	
	```nix
	let
		x = 1;
		y = 2;
	in x + y


	
	let x=1;y=2;in x+y
	```
	
	```sh
	3
	```
- ## Names and values
	- Values
		- primitive data types
		- lists
		- attribute sets
		- functions
	- ### Attribute set `{ ... }`
		- An attribute set 
			- a collection of name-value-pairs
			- names must be unique
		- Nix language =  JSON +  functions みたいな感じ
			- nix
				```nix
				{
				  string = "hello";
				  integer = 1;
				  float = 3.141;
				  bool = true;
				  null = null;
				  list = [ 1 "two" false ];
				  attribute-set = {
				    a = "hello";
				    b = 2;
				    c = 2.718;
				    d = false;
				  }; # comments are supported
				}
				```
			- json
				```json
				{
				  "string": "hello",
				  "integer": 1,
				  "float": 3.141,
				  "bool": true,
				  "null": null,
				  "list": [1, "two", false],
				  "object": {
				    "a": "hello",
				    "b": 1,
				    "c": 2.718,
				    "d": false
				  }
				}
				```
		- #### Recursive attribute set `rec { ... }`