
One way sync with rsync
-----------------------
```sh
SSH_PORT=22
USER=user
RHOST=192.168.0.2
rsync -cuhazP --stats -e "ssh -p $SSH_PORT" $USER@$RHOST:/home/$USER/path/ /home/$USER/path/
```

Sync both ways with unison
--------------------------
Install on both machines `unison`:
```sh
sudo pacman -S unison
```
Don't forget to [read the manual](https://raw.githubusercontent.com/bcpierce00/unison/refs/heads/documentation/unison-manual.txt)

Assuming the username is the same on both machines, sync two directories both ways:
```sh
DIR=your_sync_directory
RHOST=your_host
unison $DIR ssh://$RHOST/$DIR
```

With custom SSH PORT:
```sh
DIR=your_sync_directory
RHOST=your_host
unison -sshargs '-p PORT_NUM' $DIR ssh://$RHOST/$DIR
```

Press `?` for the command list.

