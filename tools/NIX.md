- Purely Functional Package manager. `treats packages as values in purely functional languages`
- All of the packages will be installed in `/nix/store` i.e: `/nix/store/b6gvzjyb2pg0kjfwrjmg1vfhh54ad73z-firefox-33.1/`
- Since each package comes with it's own has, different variants of same package can be handled
- Package management operation don't overwrite packages, just add new version in different path. i.e enabling us for rollback much more easily
```bash
nix-env --upgrade _some-packages_
nix-env --rollback
```
- `nix-env --uninstall firefox` not actually deleting it ( for rollback ). just call the garbage collector to clear up the space `nix-collect-garbage`

- all the packages are begin maintained from this repo https://github.com/NixOS/nixpkgs/tree/master
- As Entire os including kernet, app, system packages & config is built by nix-packager-manger. ===Updating system is as reliable as reinstalling===
- Because the files of a new configuration don’t overwrite old ones, you can (atomically) roll back to a previous configuration. For instance, if after a `nixos-rebuild switch` you discover that you don’t like the new configuration, you can just go bac
- fact: just copy configuration.nix and past it on another machine, run `nixox-rebuild switch` converts to yours ( whola now you have completely reproducable systesm)
- `nixos-rebuild build-vm` , spawns vm and runs the builds we can do a sanity there before applying

### Building Package from Source
- `nix-env --install package` -- tries to install it from source 

