# NixOS

Nix is a _purely functional package manager_. This means that it treats packages like values in purely functional programming languages such as Haskell — they are built by functions that don’t have side-effects, and they never change after they have been built. Nix stores packages in the _Nix store_, usually the directory `/nix/store`, where each package has its own unique subdirectory such as

`/nix/store/b6gvzjyb2pg0kjfwrjmg1vfhh54ad73z-firefox-33.1/` 

where `b6gvzjyb2pg0…` is a unique identifier for the package that captures all its dependencies (it’s a cryptographic hash of the package’s build dependency graph). This enables many powerful features.

[NixOS - NixOS Linux](https://nixos.org/)

[NixOS Manual Introduction](https://nixos.org/manual/nix/stable/introduction.html)

[Nix/Nixpkgs/NixOS · GitHub](https://github.com/NixOS)

#nix 
