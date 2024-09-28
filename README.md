# ALK's Nixvim Configuration
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

![image](https://github.com/user-attachments/assets/44ff2d6c-664c-4942-9216-7c4400f2c7a5)

## Installation

To make this configuration **persistent**, you can **modify** your `configuration.nix` file. Here’s how:

- **Create** a **variable** for your Nixvim **config** **directory** (I put it inside the **same** directory of `configuration.nix`).
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
