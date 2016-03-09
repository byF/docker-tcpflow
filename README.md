Docker TCPFLOW
==============

Have you ever wanted to inspect network TCP traffic running among containers?

Simply use [TCPFLOW](https://github.com/simsong/tcpflow) wrapped in a Docker container. This is a very light-weight image (~6.8MB) based on Alpine Linux 3.3. The repository is automatically published at Docker Hub as `byfcz/tcpflow`.

How to use it
-------------
```
docker run --net="<see below>" byfcz/tcpflow <args> 
```

You should choose your network settings based on your intent:

* [`--net host`](https://docs.docker.com/engine/reference/run/#user-defined-network) for inspecting the traffic on your host machine
* [`--net container:<name|id>`](https://docs.docker.com/engine/reference/run/#network-container) for inspecting specific container's traffic
* [`--net my-custom-net`](https://docs.docker.com/engine/reference/run/#user-defined-network) for inspecting traffic in custom networks

_Mac OSX_: Since Docker runs in a VM on OSX, using `host` Docker networking might show you only the traffic in the VM based on its network settings.

Example
-------
__WARNING__: Not using TCPFLOW's `-p` option might result in computer freeze.

Following command outputs all the traffic into console for `abc` container.
```
docker run --net="container:abc" byfcz/tcpflow -p -c
```

Default interface TCPFLOW listens to is `eth0`, you might want to change it in some case using `-i` switch. Following command shows you all the traffic on your machine concerning port 8080 on interface `eth1`; it outputs only into console and different flows have different colors:
```
docker run --net host byfcz/tcpflow -p -c -g -i eth1 port 8080
```

TCPFLOW Quick Reference
-----------------------
```
TCPFLOW version 1.4.5

usage: tcpflow [-aBcCDhJpsvVZ] [-b max_bytes] [-d debug_level]
     [-[eE] scanner] [-f max_fds] [-F[ctTXMkmg]] [-i iface] [-L semlock]
     [-m min_bytes] [-o outdir] [-r file] [-R file]
     [-S name=value] [-T template] [-w file] [-x scanner] [-X xmlfile]
      [expression]

   -a: do ALL post-processing.
   -b max_bytes: max number of bytes per flow to save
   -d debug_level: debug level; default is 1
   -f: maximum number of file descriptors to use
   -h: print this help message (-hh for more help)
   -H: print detailed information about each scanner
   -i: network interface on which to listen
   -I: generate temporal packet-> byte index files for each flow (.findex)
   -g: output each flow in alternating colors (note change!)
   -l: treat non-flag arguments as input files rather than a pcap expression
   -L  semlock - specifies that writes are locked using a named semaphore
   -p: don't use promiscuous mode
   -q: quiet mode - do not print warnings
   -r file: read packets from tcpdump pcap file (may be repeated)
   -R file: read packets from tcpdump pcap file TO FINISH CONNECTIONS
   -v: verbose operation equivalent to -d 10
   -V: print version number and exit
   -w file: write packets not processed to file
   -o  outdir   : specify output directory (default '.')
   -X  filename : DFXML output to filename
   -m  bytes    : specifies skip that starts a new stream (default 16777216).
   -F{p} : filename prefix/suffix (-hh for options)
   -T{t} : filename template (-hh for options; default %A.%a-%B.%b%V%v%C%c)
   -Z: do not decompress gzip-compressed HTTP transactions

Control of Scanners:
   -E scanner   - turn off all scanners except scanner
   -S name=value  Set a configuration parameter (-hh for info)

Settable Options (and their defaults):
   -S enable_report=YES    Enable report.xml ()
   -S http_cmd=    Command to execute on each HTTP attachment (http)
   -S http_alert_fd=-1    File descriptor to send information about completed HTTP attachments (http)
   -S tcp_timeout=0    Timeout for TCP connections (tcpdemux)
   -S check_fcs=YES    Require valid Frame Check Sum (FCS) (wifiviz)

   -e http - enable scanner http
   -e md5 - enable scanner md5
   -e netviz - enable scanner netviz
   -e wifiviz - enable scanner wifiviz

   -x tcpdemux - disable scanner tcpdemux
Console output options:
   -B: binary output, even with -c or -C (normally -c or -C turn it off)
   -c: console print only (don't create files)
   -C: console print only, but without the display of source/dest header
   -0: don't print newlines after packets when printing to console   -s: strip non-printable characters (change to '.')
   -D: output in hex (useful to combine with -c or -C)

Rendering not available because Cairo was not installed.

expression: tcpdump-like filtering expression

See the man page for additional information.
```