Problem: no audio on Dell 5690

```sh
↪ aplay -l
aplay: device_list:279: no soundcards found...

↪ journalctl -p 3 | grep audio

Jul 02 14:09:38 voytpc kernel: sof-audio-pci-intel-mtl 0000:00:1f.3: SOF firmware and/or topology file not found.
Jul 02 14:09:38 voytpc kernel: sof-audio-pci-intel-mtl 0000:00:1f.3: error: sof_probe_work failed err: -2
```

How to resolve:
```sh
sudo pacman -S sof-firmware sof-tools alsa-ucm-conf
```
sof-tools is optional
