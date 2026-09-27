

```sh
#!/bin/sh

SSH_PORT=22
IP=192.168.0.100

while true; do
  mutagen sync list | grep Identifier | cut -d':' -f2 | while IFS= read -r line; do
    echo "Killing connection: $line"
    mutagen sync terminate $line
  done
  mutagen sync list | grep 'No synchronization sessions found' && break
  sleep 1
done

mutagen sync create \
  --mode=one-way-replica \
  --name=voytpc \
  ~/p/linux/ \
  voyt@$IP:$SSH_PORT:/home/voyt/p/linux \
  --ignore=.out

mutagen sync list
mutagen sync flush voytpc
```
