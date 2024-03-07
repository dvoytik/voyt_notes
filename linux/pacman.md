pacman
======

Check integrity of installed packages and repair if broken are found
--------------------------------------------------------------------

```sh
sudo pacman -S pacutils
sudo paccheck --list-broken --recursive --files --sha256sum --md5sum > broken_packages.txt
sudo pacman -S $(cat broken_packages.txt)
```

Check when a package was installed
----------------------------------
```sh
pacman -Qi package_name | grep Install
```
