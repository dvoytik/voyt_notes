
How to enable systemd-resolved in stub mode

```
sudo pacman -Rs openresolv

sudo systemctl enable systemd-resolved.service
sudo systemctl restart systemd-resolved.service
sudo ln -sf ../run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
sudo systemctl restart systemd-resolved.service
nmcli networking off
nmcli networking on
```

Test:
```
sudo systemctl status systemd-resolved.service
resolvectl status
resolvectl query archlinux.org
```

TODO: check:
```
ResolveUnicastSingleLabel=yes
```
in `/etc/systemd/resolved.conf`


Source: https://wiki.archlinux.org/title/Systemd-resolved#DNS
