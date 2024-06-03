#Nix #nixos 
# 時系列で学ぶNixのエコシステム
- Hook
	- Nixは難易度が高い。
	- なんとなく動かしてみてるが、意味がわからない。
	- infinite recursion という名の渦に飲まれた屍たち
	- すごく便利そうなものがたくさんあってoverwhelmed
	- その機能たちが生まれた経緯をしらないから、使ってもよく分からない。
- Time line
	- 2003年
		- Nix
			- Nix Shell
			- Store
			- Derivations
			- Nix expressions
			- Attribute Set
			- Channels
			- Profiles
	- 2006年
		- NixOS
		- 論文発表
			-  [The Purely Functional Software Deployment Model, 2006, Eelco Dolstra](https://grosskurth.ca/bib/2006/dolstra-thesis.pdf)
	- 2008
		- Hydra
	- 2012
		- Nixpkgs
			```bash
			$ curl https://api.github.com/repos/NixOS/nixpkgs | grep "created_at"
			
			"created_at": "2012-06-04T02:49:46Z",
			```	
	- 2017
		- Home Manager
			```bash
			$ curl https://api.github.com/repos/nix-community/home-manager | grep "created_at"
			
			  "created_at": "2017-01-14T12:11:06Z",
			```	
			
	- 2020年
		- Flakes
