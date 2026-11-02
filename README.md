# opencode-nix

Nix flake for [OpenCode](https://github.com/anomalyco/opencode), packaged from the official GitHub release binaries.

A GitHub Actions workflow checks once a day for a new OpenCode release and commits the updated `version.json` automatically.

Supports `x86_64-linux`, `aarch64-linux`, `x86_64-darwin` and `aarch64-darwin`.

## Usage

Replace `OWNER` with your GitHub username.

```nix
{
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    opencode-nix.url = "github:OWNER/opencode-nix";
  };
}
```

NixOS / Home Manager:

```nix
{ inputs, pkgs, ... }:
{
  environment.systemPackages = [   # or home.packages
    inputs.opencode-nix.packages.${pkgs.stdenv.hostPlatform.system}.default
  ];
}
```

Or via the overlay (`opencode-nix.overlays.default`), which provides `pkgs.opencode`.

Try it without installing:

```sh
nix run github:OWNER/opencode-nix
```

## Updating

```sh
nix flake update opencode-nix
nixos-rebuild switch  # or home-manager switch
```

## Manual update

```sh
nix develop
./update.sh           # check only
./update.sh --update  # write new version and hashes to version.json
```
