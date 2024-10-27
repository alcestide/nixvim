# ALK's Nixvim Configuration
[![License](https://img.shields.io/github/license/alcestide/nixvim)](https://en.wikipedia.org/wiki/Unlicense)
[![Written in Nix](https://img.shields.io/badge/code-nix-blue)](https://nixos.org/)
[![NixOS Unstable](https://img.shields.io/badge/NixOS-24.05-blue.svg?style=flat-square&logo=NixOS&logoColor=white)](https://nixos.org)
[![GitHub last commit](https://img.shields.io/github/last-commit/alcestide/nixvim)](#)
[![Free](https://img.shields.io/badge/free_for_non_commercial_use-brightgreen)](#-license)
---
## Testing

You can **try out** my configuration by running the **following** **command** in your **shell**:
```nix
nix run github:alcestide/nixvim
```
Alternatively, you could:
```
git clone https://github.com/alcestide/nixvim
``` 
and then run `nix run .` inside the cloned directory.

![Screenshot_20241027-125629](https://github.com/user-attachments/assets/820ff294-83c8-40bd-ab6f-7e0957c62517)

![Screenshot_20241027-125520](https://github.com/user-attachments/assets/3dffd9d7-f9fa-402f-bb48-312bf88d5090)

![Screenshot_20241027-125535](https://github.com/user-attachments/assets/610fd68e-aacb-4dc9-9e44-59c590401ba1)

## Installation
To make this configuration **persistent**, you can either use **flakes** (easier) or the **standalone** method. Here’s how:

### Flakes
- In your **flake.nix**, create inside `inputs`:
```nix
    nixvim = {
      url = "github:alcestide/nixvim";
    };
```
Then simply add it to your **packages** like this:
```nix
    inputs.nixvim.packages."x86_64-linux".default
```

### Standalone

- **Create** a **variable** for your Nixvim **config** **directory** (I put mine inside the **same** directory of `configuration.nix`).
- **Create** another **variable** and assign to it `nixvim.legacyPackages."${pkgs.stdenv.hostPlatform.system}"` (in this case the variable is named "**alknix**", but you can call it as you like).
- Pass the Nixvim **config** as a **module** to the `makeNixvimWithModule` function as shown below:
```nix
let
	nixvim-config = import ./nixvim/config;
alknixvim = nixvim.legacyPackages.x86_64-linux.makeNixvimWithModule {
	inherit pkgs;
	module = nixvim-config;};
in
```
**Add** the newly created **package** to your `systemPackages` list:

```nix
environment.systemPackages = [
    alknixvim
];
```

Finally, **rebuild** your NixOS configuration by running:
`sudo nixos-rebuild switch`
This should set everything up.
If you encounter **issues**:

- Check for version **mismatches** between `nixvim` and `nixpkgs` inside your `flake.nix`. **Make sure they're both the same version!**
- **Review** the **logs** for any **errors** that might indicate what’s going wrong.
