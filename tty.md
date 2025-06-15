# tty

## History

Long (but getting shorter) and interesting reads about what and why

* The TTY demystified - <https://www.linusakesson.net/programming/tty/>
* TTY: under the hood - <https://www.yabage.me/2016/07/08/tty-under-the-hood/>
* Terminal under the hood - TTY & PTY - <https://yakout.io/blog/terminal-under-the-hood/>

TTYs ("Teletypes") predate computers! They were fax machines that got repurposed
to talk to computers when computers came along. Maybe interesting context for
the mindscape Turing was in when he came up with his test.

### Seat

"Seat" is an archaic term (but who knows, maybe we'll return to that style of
computing in the future) when the computer occupied a room, and there were
multiple seats in the room, each with a keyboard, mouse and a TTY. Currently we
usually have a single "seat" (keyboard, mouse and display combination), but
multiple TTYs (both "real" ones, the gettys launched by init on system startup,
and "pseudo" ones, one for each terminal emulator
([xfce4-terminal](xterm). Simulacra and Simulation).


```sh
ps -ef | grep '[g]etty'
```
```
root      2494     1  0 08:53 tty1     00:00:00 /sbin/getty 38400 tty1
root      2495     1  0 08:53 tty2     00:00:00 /sbin/getty 38400 tty2
root      2497     1  0 08:53 tty3     00:00:00 /sbin/getty 38400 tty3
root      2498     1  0 08:53 tty4     00:00:00 /sbin/getty 38400 tty4
root      2502     1  0 08:53 tty5     00:00:00 /sbin/getty 38400 tty5
root      2503     1  0 08:53 tty6     00:00:00 /sbin/getty 38400 tty6
root      2504     1  0 08:53 ?        00:00:00 /sbin/getty -L 0 ttyAMA0 vt100
```

### pty

pty is a user space terminal (they're all pseudo, the ttys too, but these are
pseudo a level deeper).


```sh
tty
```
```
/dev/pts/9
```
