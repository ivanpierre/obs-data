# NixOps

[GitHub - NixOS/nixops: NixOps is a tool for deploying to NixOS machines in a network or cloud.](https://github.com/NixOS/nixops)
_NixOps_ is a tool for deploying to [NixOS](https://nixos.org/) machines in a network or the cloud. Key features include:

-   **Declarative**: NixOps determines and carries out actions necessary to realize a deployment configuration.
-   **Testable**: Try your deployments on [VirtualBox](https://github.com/nix-community/nixops-vbox) or [libvirtd](https://github.com/nix-community/nixops-libvirtd).
-   **Multi Cloud Support**: Currently supports deployments to [AWS](https://github.com/NixOS/nixops-aws), [Hetzner](https://github.com/NixOS/nixops-hetzner), and [GCE](https://github.com/AmineChikhaoui/nixops-gce)
-   **Separation of Concerns**: Deployment descriptions are divided into _logical_ and _physical_ aspects. This makes it easy to separate parts that say _what_ a machine should do from _where_ they should do it.
-   **Extensible**: _NixOps_ is extensible through a plugin infrastructure which can be used to provide additional backends.

[NixOps User's Guide](https://hydra.nixos.org/build/115931128/download/1/manual/manual.html)