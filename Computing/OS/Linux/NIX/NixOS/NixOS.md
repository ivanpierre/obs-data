# NixOS

Nix is a _purely functional package manager_. This means that it treats packages like values in purely functional programming languages such as Haskell — they are built by functions that don’t have side-effects, and they never change after they have been built. Nix stores packages in the _Nix store_, usually the directory `/nix/store`, where each package has its own unique subdirectory such as

`/nix/store/b6gvzjyb2pg0kjfwrjmg1vfhh54ad73z-firefox-33.1/` 

where `b6gvzjyb2pg0…` is a unique identifier for the package that captures all its dependencies (it’s a cryptographic hash of the package’s build dependency graph). This enables many powerful features.

- [NixOS - NixOS Linux](https://nixos.org/)
- [NixOS Manual Introduction](https://nixos.org/manual/nix/stable/introduction.html)
- [NixOS Wiki](https://nixos.wiki/)
- [NixOS Discourse - NixOS community forum](https://discourse.nixos.org/)
- [Nix Expression Language - NixOS Wiki](https://nixos.wiki/wiki/Nix_Expression_Language)
- [NixOS - Nix Pills](https://nixos.org/guides/nix-pills/)
- [Nix/Nixpkgs/NixOS · GitHub](https://github.com/NixOS)
- [GitHub - nix-community/awesome-nix: 😎 A curated list of the best resources in the Nix community [maintainer=@houstdav000]](https://github.com/nix-community/awesome-nix)
- [NixOS - Nixpkgs stable manual](https://nixos.org/manual/nixpkgs/stable/)
- [NixOS - Nixpkgs unstable manual](https://nixos.org/manual/nixpkgs/unstable/)
- [Nix Manual Stable](https://nixos.org/manual/nix/stable/)
- [Nix Manual Unstable](https://nixos.org/manual/nix/unstable/)

#nix 
