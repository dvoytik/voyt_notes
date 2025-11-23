
First step - ping:
```
ping -c10 -w10 ${SERVER_ADDR}
```
`-w10` - wait 10 seconds
`-c10` - send 10 ICMP packets

The "Ping Sweep" test for cases when larger MTU can cause packet loss:

```
ping -M do -s 1472 ${SERVER_ADDR}
```

Test with TCP connection to the port 80:
```
nc -zv nv 80
```
-z      Only scan for listening daemons, without sending any data to them.  Cannot be used together with -l.
-v      Produce more verbose output.


```
↪ nc -zv archlinux.org 80
Connection to archlinux.org (2a01:4f9:c012:16e3::1) 80 port [tcp/http] succeeded!

↪ nc -zv archlinux.org 90
nc: connect to archlinux.org (2a01:4f9:c012:16e3::1) port 90 (tcp) failed: Permission denied
nc: connect to archlinux.org (46.62.203.164) port 90 (tcp) failed: No route to host

↪ nc -zv localhost 90
nc: connect to localhost (::1) port 90 (tcp) failed: Connection refused
nc: connect to localhost (127.0.0.1) port 90 (tcp) failed: Connection refused
```

Test with tcpdump:
```
sudo tcpdump -i any host ${SERVER_ADDR} -w debug.pcap
```

Check global TCP stats in Kernel:
```
watch -d 'netstat -s | grep -E "retransmitted|dropped|error"'
```
Look for "segments retransmitted" increasing rapidly.

Interface Errors:
```
ip -s link show eth0
```
Look for RX errors, dropped, or overruns. If overruns are high, your network card's hardware buffer is full.


