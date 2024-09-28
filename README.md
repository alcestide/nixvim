# ALK's Nixvim Configuration
## Testing

You can **try out** my configuration by simply running inside your shell the following **command**:

```nix
nix run github:alcestide/nixvim
```
or you can 
```bash
git clone https://github.com/alcestide/nixvim
``` 
and use `nix run .` inside the directory.

![image](https://github.com/user-attachments/assets/44ff2d6c-664c-4942-9216-7c4400f2c7a5)

## Installation
Make sure **not** to import **mismatched** `nixvim` and `nixpkgs` **versions** inside your `flake.nix`! 

I personally make my configuration **persistent** from inside my `configuration.nix` by creating variables for my nixvim **config** **directory** and **passing** it to the `makeNixvimWithModule` **function**.
```nix
let
	nixvim-config = import ./nixvim/config;
alknixvim = nixvim.legacyPackages.x86_64-linux.makeNixvimWithModule {
	inherit pkgs;
	module = nixvim-config;};
in
```
and then add the **variable** name inside `systemPackages` as a **package**.
```nix
environment.systemPackages = [
    alknixvim
];
```
