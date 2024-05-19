#nix #nixos #flakes

- NixOSはシステムレベルの設定のみを管理する
- ホームディレクトリ内の管理は管轄外
- home-managerがユーザレベルの設定を管理できる。
    - ホームディレクトリ

- home.nix vs configuration.nix
    - configuration.nix
        - すべてのユーザーに必要な設定
    - home.nix
        - ユーザ単位で必要な設定。
