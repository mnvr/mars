# Tools

## netstat

List open ports.

```sh
netstat -ntup
```

* `-n` - show port number, don't try to resolve it to a service.
* `-t` tcp, `-u` udp.
* `-p` - show the program.
* `-l` can be used to restrict it to listeners.

We get it from busybox.

```sh
readlink `which netstat`
```
```
/bin/busybox
```
