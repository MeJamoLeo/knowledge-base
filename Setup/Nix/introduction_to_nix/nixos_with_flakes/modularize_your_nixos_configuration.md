NixOS設定のモジュール化
この時点で、システム全体のスケルトンが構成されています。/etc/nixosの現在の構成は次のようになっています：

arduino
Copy code
$ tree
.
├── flake.lock
├── flake.nix
├── home.nix
└── configuration.nix
これらの4つのファイルの機能は以下の通りです：

flake.lock: 自動生成されたバージョンロックファイルで、すべての入力ソース、ハッシュ値、およびフレークのバージョン番号を記録し、再現性を保証します。
flake.nix: sudo nixos-rebuild switchを実行する際に認識され、デプロイされるエントリファイルです。flake.nixのすべてのオプションについてはNixOS WikiのFlakesを参照してください。
configuration.nix: flake.nixにNixモジュールとしてインポートされ、現在すべてのシステムレベルの設定がここに書かれています。configuration.nixのすべてのオプションについてはNixOSマニュアルを参照してください。
home.nix: Home-Managerによってユーザーryanの設定としてflake.nixにインポートされ、ryanのホームフォルダーのすべての設定を含みます。home.nixのすべてのオプションについてはHome-Managerのオプションを参照してください。
これらのファイルを修正することで、システムおよびホームディレクトリの状態を宣言的に変更できます。

しかし、設定が増えると、configuration.nixやhome.nixに依存するだけではファイルが膨れ上がり、管理が難しくなります。より良い解決策は、Nixモジュールシステムを使用して設定を複数のNixモジュールに分割し、分類して記述することです。

Nixモジュールシステムは、importsというパラメータを提供し、.nixファイルのリストを受け入れて、これらのファイルに定義されたすべての設定を現在のNixモジュールにマージします。importsは単に重複する設定を上書きするのではなく、より合理的に処理します。例えば、複数のモジュールでprogram.packages = [...]が定義されている場合、importsはすべてのNixモジュールで定義されたprogram.packagesを1つのリストにマージします。属性セットも正しくマージされます。具体的な動作は自分で確認してみてください。

importsの説明はNixpkgs-Unstable公式マニュアル - evalModules Parametersで見つけましたが、少し曖昧です...

importsを利用することで、home.nixやconfiguration.nixを複数の.nixファイルに分割することができます。例として、packages.nixというモジュールを見てみましょう：

nix
Copy code
{
  config,
  pkgs,
  ...
}: {
  imports = [
    (import ./special-fonts-1.nix {inherit config pkgs;}) # (1)
    ./special-fonts-2.nix # (2)
  ];

  fontconfig.enable = true;
}
このモジュールは、special-fonts-1.nixとspecial-fonts-2.nixという2つの他のモジュールを読み込みます。両方のファイルは次のように見えるモジュールです。

nix
Copy code
{ config, pkgs, ...}: {
    # 設定内容 ...
}
上記の2つのインポート文は受け取るパラメータにおいて同等です：

ステートメント（1）は、special-fonts-1.nixの関数をインポートし、{config = config; pkgs = pkgs}を渡して呼び出します。基本的には、importsリスト内で呼び出しの戻り値（別の部分的な設定属性セット）を使用します。
ステートメント（2）はモジュールへのパスを定義し、Nixは設定を組み立てる際に関数を自動的にロードします。これにより、packages.nixの関数から一致する引数がロードされたspecial-fonts-2.nixの関数に渡され、結果的にimport ./special-fonts-2.nix {config = config; pkgs = pkgs}となります。
モジュール化の良い出発点として以下をお勧めします：

Misterio77/nix-starter-configs
より複雑な例として、ryan4yin/nix-config/i3-kickstarterは、i3ウィンドウマネージャーを使用した以前のNixOSシステムの設定です。その構成は次の通りです：

shell
Copy code
├── flake.lock
├── flake.nix
├── home
│   ├── default.nix         # ここでimports = [...]で全てのサブモジュールをインポート
│   ├── fcitx5              # fcitx5入力メソッドの設定
│   │   ├── default.nix
│   │   └── rime-data-flypy
│   ├── i3                  # i3ウィンドウマネージャーの設定
│   │   ├── config
│   │   ├── default.nix
│   │   ├── i3blocks.conf
│   │   ├── keybindings
│   │   └── scripts
│   ├── programs
│   │   ├── browsers.nix
│   │   ├── common.nix
│   │   ├── default.nix   # ここでprogramsフォルダー内の全てのモジュールをimports = [...]でインポート
│   │   ├── git.nix
│   │   ├── media.nix
│   │   ├── vscode.nix
│   │   └── xdg.nix
│   ├── rofi              # rofiランチャーの設定
│   │   ├── configs
│   │   │   ├── arc_dark_colors.rasi
│   │   │   ├── arc_dark_transparent_colors.rasi
│   │   │   ├── power-profiles.rasi
│   │   │   ├── powermenu.rasi
│   │   │   ├── rofidmenu.rasi
│   │   │   └── rofikeyhint.rasi
│   │   └── default.nix
│   └── shell             # シェル/ターミナル関連の設定
│       ├── common.nix
│       ├── default.nix
│       ├── nushell
│       │   ├── config.nu
│       │   ├── default.nix
│       │   └── env.nu
│       ├── starship.nix
│       └── terminals.nix
├── hosts
│   ├── msi-rtx4090      # メインマシンの設定
│   │   ├── default.nix  # これは古いconfiguration.nixだが、内容のほとんどはモジュールに分割されている。
│   │   └── hardware-configuration.nix  # ハードウェアおよびディスク関連の設定、自動生成される
│   └── my-nixos       # テストマシンの設定
│       ├── default.nix
│       └── hardware-configuration.nix
├── modules          # 再利用可能な共通のNixOSモジュール
│   ├── i3.nix
│   └── system.nix
└── wallpaper.jpg    # 壁紙
上記の構成に従う必要はなく、自分の好きな方法で設定を整理できます。重要なのは、importsを使用してすべてのサブモジュールをメインモジュールにインポートすることです。

lib.mkOverride, lib.mkDefault, lib.mkForce
Nixでは、一部の人がlib.mkDefaultやlib.mkForceを使用して値を定義します。これらの関数はオプションのデフォルト値や強制値を設定するために設計されています。

lib.mkDefaultとlib.mkForceのソースコードを確認するには、nix repl -f '<nixpkgs>'を実行し、次に:e lib.mkDefaultを入力します。nix replの詳細については、:?を入力してヘルプ情報を参照してください。

ソースコードは以下の通りです：

nix
Copy code
  # ......

  mkOverride = priority: content:
    { _type = "override";
      inherit priority content;
    };

  mkOptionDefault = mkOverride 1500; # オプションデフォルトの優先度
  mkDefault = mkOverride 1000; # 非ユーザーモジュールの設定セクションでデフォルトを設定するために使用
  mkImageMediaOverride = mkOverride 60; # ホスト設定に含まれるイメージメディアプロファイルはホスト設定を上書きする必要があるが、ユーザーがmkForceすることを許可する必要がある
  mkForce = mkOverride 50;
  mkVMOverride = mkOverride 10; # ‘nixos-rebuild build-vm’で使用される

  # ......
まとめると、lib.mkDefaultは内部的に優先度1000でオプションのデフォルト値を設定し、lib.mkForceは内部的に優先度50でオプションの値を強制します。オプションの値を直接設定すると、デフォルトの優先度1000で設定され、lib.mkDefaultと同じになります。

優先度値が低いほど、実際の優先度は高くなります。その結果、lib.mkForceはlib.mkDefaultよりも高い優先度を持ちます。同じ優先度で複数の値を定義すると、Nixはエラーをスローします。

これらの関数を使用すると、設定のモジュール化に非常に役立ちます。低レベルのモジュール（ベースモジュール）でデフォルト値を設定し、高レベルのモジュールで値を強制することができます。

例えば、ryan4yin/nix-config/blob/c515ea9/modules/nixos/core-server.nixの設定では、次のようにデフォルト値を定義しています：

nix{6}
Copy code
{ lib, pkgs, ... }:

{
  # ......

  nixpkgs.config.allowUnfree = lib.mkDefault false;

  # ......
}
次に、デスクトップマシン用にryan4yin/nix-config/blob/c515ea9/modules/nixos/core-desktop.nixで値を上書きしています：

nix{10}
Copy code
{ lib, pkgs, ... }:

{
  # ベースモジュールをインポート
  imports = [
    ./core-server.nix
  ];

  # ベースモジュールで定義されたデフォルト値を上書き
  nixpkgs.config.allowUnfree = lib.mkForce true;

  # ......
}
lib.mkOrder, lib.mkBefore, lib.mkAfter
lib.mkDefaultとlib.mkForceに加えて、lib.mkBeforeやlib.mkAfterもあり、リスト型オプションのマージ順序を設定するために使用されます。これらの関数は設定のモジュール化にさらに寄与します。

リスト型オプションに関する公式ドキュメントは見つかりませんでしたが、マージ結果がマージ順序に関連するタイプだと理解しています。この理解に基づくと、listおよびstringタイプがリスト型オプションであり、これらの関数は実際にこれらのタイプで使用できます。

前述のように、同じ優先度で複数の値を定義すると、Nixはエラーをスローします。しかし、lib.mkOrder、lib.mkBefore、またはlib.mkAfterを使用すると、同じ優先度で複数の値を定義でき、指定した順序でマージされます。

lib.mkBeforeのソースコードを確認するには、nix repl -f '<nixpkgs>'を実行し、次に:e lib.mkBeforeを入力します。nix replの詳細については、:?を入力してヘルプ情報を参照してください：

nix
Copy code
  # ......

  mkOrder = priority: content:
    { _type = "order";
      inherit priority content;
    };

  mkBefore = mkOrder 500;
  defaultOrderPriority = 1000;
  mkAfter = mkOrder 1500;

  # ......
したがって、lib.mkBeforeはlib.mkOrder 500の略であり、lib.mkAfterはlib.mkOrder 1500の略です。

lib.mkBeforeとlib.mkAfterの使用方法をテストするために、簡単なフレークプロジェクトを作成しましょう：

nix{10
Copy code
# flake.nix
{
  inputs.nixpkgs.url = "github:NixOS/nixpkgs/nixos-23.11";
  outputs = {nixpkgs, ...}: {
    nixosConfigurations = {
      "my-nixos" = nixpkgs.lib.nixosSystem {
        system = "x86_64-linux";

        modules = [
          ({lib, ...}: {
            programs.bash.shellInit = lib.mkBefore ''
              echo 'insert before default'
            '';
            programs.zsh.shellInit = lib.mkBefore "echo 'insert before default';";
            nix.settings.substituters = lib.mkBefore [
              "https://nix-community.cachix.org"
            ];
          })

          ({lib, ...}: {
            programs.bash.shellInit = lib.mkAfter ''
              echo 'insert after default'
            '';
            programs.zsh.shellInit = lib.mkAfter "echo 'insert after default';";
            nix.settings.substituters = lib.mkAfter [
              "https://ryan4yin.cachix.org"
            ];
          })

          ({lib, ...}: {
            programs.bash.shellInit = ''
              echo 'this is default'
            '';
            programs.zsh.shellInit = "echo 'this is default';";
            nix.settings.substituters = [
              "https://nix-community.cachix.org"
            ];
          })
        ];
      };
    };
  };
}
上記のフレークには、マルチライン文字列、シングルライン文字列、およびリストのマージ順序にlib.mkBeforeとlib.mkAfterを使用する例が含まれています。結果をテストしましょう：

bash
Copy code
# 例1: マルチライン文字列のマージ
› echo $(nix eval .#nixosConfigurations.my-nixos.config.programs.bash.shellInit)
trace: warning: system.stateVersion is not set, defaulting to 23.11. Read why this matters on https://nixos.org/manual/nixos/stable/options.html#opt-system.stateVersio
n.
"echo 'insert before default'

echo 'this is default'

if [ -z \"$__NIXOS_SET_ENVIRONMENT_DONE\" ]; then
 . /nix/store/60882lm9znqdmbssxqsd5bgnb7gybaf2-set-environment
fi



echo 'insert after default'
"

# 例2: シングルライン文字列のマージ
› echo $(nix eval .#nixosConfigurations.my-nixos.config.programs.zsh.shellInit)
"echo 'insert before default';
echo 'this is default';
echo 'insert after default';"

# 例3: リストのマージ
› nix eval .#nixosConfigurations.my-nixos.config.nix.settings.substituters
[ "https://nix-community.cachix.org" "https://nix-community.cachix.org" "https://cache.nixos.org/" "https://ryan4yin.cachix.org" ]

このように、lib.mkBeforeとlib.mkAfterを使用すると、マルチライン文字列、シングルライン文字列、およびリストのマージ順序を定義できます。マージの順序は定義の順序と同じです。

モジュールシステムのより深い紹介については、Module System & Custom Optionsを参照してください。

参考文献
Nix modules: Improving Nix's discoverability and usability
Module System - Nixpkgs
