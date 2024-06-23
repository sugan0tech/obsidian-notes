Installation: 
`curl -L https://nixos.org/nix/install | sh -s -- --daemon `
- Nix requires a demon service


What is nix: 
- A `FlatPack` like package manager, can easy package management i.e all the packages under `/nix/store/`
- To spawn a nix-shell just run `nix-shell -p`
- To spawn a nix-shell with in-session package `nix-shell -p pfetch`
	- connects to nix-shell, with `pfetch` installed



Uninstall:
- There will be multiple nix-build user will be created along with `nixbld` user group, need to remove those with this following script
```bash
for i in $(seq 1 9); do
  sudo userdel nixbld0$i
done
sudo groupdel nixbld
```