# Network troubleshooting tools

The typical tools are:

- ifconfig
- netstat
- traceroute

but these are being replaced with newever versions

- ip
- ss

ifconfig

``` bash
enp0s25: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 68:f7:68:22:57:e2  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 1  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 20  memory 0xf0600000-f0620000  

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 378556  bytes 148045905 (148.0 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 378556  bytes 148045905 (148.0 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

tun0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1412
        inet 172.16.20.189  netmask 255.255.255.192  destination 172.16.20.189
        inet6 fe80::a20e:378c:56ec:e9ef  prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 500  (UNSPEC)
        RX packets 5236  bytes 3518773 (3.5 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 5505  bytes 425212 (425.2 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlp3s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.10.52  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::e222:874e:54ff:e55b  prefixlen 64  scopeid 0x20<link>
        ether e8:b1:fc:73:19:3c  txqueuelen 1000  (Ethernet)
        RX packets 13499908  bytes 14418166463 (14.4 GB)
        RX errors 0  dropped 126476  overruns 0  frame 0
        TX packets 5010098  bytes 1377667376 (1.3 GB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

netstat (-a all, -n no dns resolution, -t tcp ports)

``` bash
netstat -ant
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
tcp        0    216 192.168.10.18:22        192.168.10.52:47152     ESTABLISHED
tcp6       0      0 :::22                   :::*                    LISTEN  

```

netstat (-a all, -n no dns resolution, -u udp ports)

``` bash
netstat -anu
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
udp        0      0 0.0.0.0:68              0.0.0.0:*                          
udp        0      0 0.0.0.0:161             0.0.0.0:*                          
udp        0      0 0.0.0.0:1194            0.0.0.0:*                          
udp        0      0 0.0.0.0:43239           0.0.0.0:*                          
udp        0      0 0.0.0.0:5353            0.0.0.0:*                          
udp6       0      0 :::546                  :::*                               
udp6       0      0 :::5353                 :::*                               
udp6       0      0 :::42351                :::* 
```

netstat (-a all, -n no dns resolution, -t tcp ports, -p process (needs sudo or root) )

``` bash
sudo netstat -antp
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      641/sshd            
tcp        0    196 192.168.10.18:22        192.168.10.52:47152     ESTABLISHED 28098/sshd: pbultee 
tcp6       0      0 :::22                   :::*                    LISTEN      641/sshd            

```

But these are being replaced with different ones like `ip addr show` and `ss`

ip addr show

``` bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s25: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether 68:f7:68:02:57:e2 brd ff:ff:ff:ff:ff:ff
3: wlp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether e8:b1:fc:73:19:3c brd ff:ff:ff:ff:ff:ff
    inet 192.168.10.52/24 brd 192.168.1.255 scope global dynamic noprefixroute wlp3s0
       valid_lft 3380sec preferred_lft 3380sec
    inet6 fe80::e222:874e:54ff:e55b/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
26: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1412 qdisc fq_codel state UNKNOWN group default qlen 500
    link/none 
    inet 172.16.20.189/26 brd 172.16.20.191 scope global noprefixroute tun0
       valid_lft forever preferred_lft forever
    inet6 fe80::ad9d:c988:ff1:f76d/64 scope link stable-privacy 
       valid_lft forever preferred_lft forever
```

You might want to get stats so use `ip -s link`

``` bash
ip -s link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    RX: bytes  packets  errors  dropped overrun mcast   
    84795127   249123   0       0       0       0       
    TX: bytes  packets  errors  dropped carrier collsns 
    84795127   249123   0       0       0       0       
2: enp0s25: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN mode DEFAULT group default qlen 1000
    link/ether 68:f7:28:02:57:e2 brd ff:ff:ff:ff:ff:ff
    RX: bytes  packets  errors  dropped overrun mcast   
    0          0        0       0       0       0       
    TX: bytes  packets  errors  dropped carrier collsns 
    0          0        0       0       0       0       
3: wlp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DORMANT group default qlen 1000
    link/ether e8:b1:fc:73:19:3c brd ff:ff:ff:ff:ff:ff
    RX: bytes  packets  errors  dropped overrun mcast   
    4903238146 5769516  0       66533   0       0       
    TX: bytes  packets  errors  dropped carrier collsns 
    489929462  2067823  0       0       0       0       

```

Using `ss` with the same -a -n -t -u and -p options

``` bash
sudo ss -antup
Netid      State       Recv-Q       Send-Q             Local Address:Port              Peer Address:Port                                                                     
udp        UNCONN      0            0                        0.0.0.0:68                     0.0.0.0:*           users:(("dhcpcd",pid=626,fd=10))                             
udp        UNCONN      0            0                        0.0.0.0:161                    0.0.0.0:*           users:(("snmpd",pid=636,fd=8))                               
udp        UNCONN      0            0                        0.0.0.0:1194                   0.0.0.0:*           users:(("openvpn",pid=627,fd=6))                             
udp        UNCONN      0            0                        0.0.0.0:43239                  0.0.0.0:*           users:(("avahi-daemon",pid=351,fd=14))                       
udp        UNCONN      0            0                        0.0.0.0:5353                   0.0.0.0:*           users:(("avahi-daemon",pid=351,fd=12))                       
udp        UNCONN      0            0                              *:546                          *:*           users:(("dhcpcd",pid=626,fd=15))                             
udp        UNCONN      0            0                              *:5353                         *:*           users:(("avahi-daemon",pid=351,fd=13))                       
udp        UNCONN      0            0                              *:42351                        *:*           users:(("avahi-daemon",pid=351,fd=15))                       
tcp        LISTEN      0            128                      0.0.0.0:22                     0.0.0.0:*           users:(("sshd",pid=641,fd=3))                                
tcp        ESTAB       0            0                   192.168.10.18:22              192.168.10.52:47152       users:(("sshd",pid=28115,fd=3),("sshd",pid=28098,fd=3))      
tcp        LISTEN      0            128                         [::]:22                        [::]:*           users:(("sshd",pid=641,fd=4))
```

using `lsof`

``` bash
sudo lsof -i -n
COMMAND    PID        USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
avahi-dae  351       avahi   12u  IPv4   14807      0t0  UDP *:mdns 
avahi-dae  351       avahi   13u  IPv6   14808      0t0  UDP *:mdns 
avahi-dae  351       avahi   14u  IPv4   14809      0t0  UDP *:43239 
avahi-dae  351       avahi   15u  IPv6   14810      0t0  UDP *:42351 
dhcpcd     626        root   10u  IPv4   14857      0t0  UDP *:bootpc 
dhcpcd     626        root   15u  IPv6   15710      0t0  UDP *:dhcpv6-client 
openvpn    627      nobody    6u  IPv4   17768      0t0  UDP *:openvpn 
snmpd      636 Debian-snmp    8u  IPv4   16025      0t0  UDP *:snmp 
sshd       641        root    3u  IPv4   15949      0t0  TCP *:ssh (LISTEN)
sshd       641        root    4u  IPv6   15951      0t0  TCP *:ssh (LISTEN)
sshd      3793        root    3u  IPv4 3217011      0t0  TCP 192.168.10.18:ssh->192.168.10.52:55170 (ESTABLISHED)
sshd      3799    pbulteel    3u  IPv4 3217011      0t0  TCP 192.168.10.18:ssh->192.168.10.52:55170 (ESTABLISHED)
```

If you are in a bind and still want to get some stats without any of those tools, look at /proc/net/dev

``` bash
cat /proc/net/dev
Inter-|   Receive                                                |  Transmit
 face |bytes    packets errs drop fifo frame compressed multicast|bytes    packets errs drop fifo colls carrier compressed
wlp3s0: 4091551036 4988130    0 58927    0     0          0         0 441568986 1830581    0    0    0     0       0          0
enp0s25:       0       0    0    0    0     0          0         0        0       0    0    0    0     0       0          0
    lo: 80298580  232744    0    0    0     0          0         0 80298580  232744    0    0    0     0       0          0
```

## DNS caching

Sometimes you are going to a website or server whose ip address AND DNS entry has changed, but your applications still go to the wrong IP. This would be due to DNS Caching. 

Example

``` bash
ping server.example.com
PING server.example.com(192.168.1.10)
^C
--- server.example.com ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 2032ms

dig @ns.example.com server.example.com
<snip>
server.example.com.     300     IN      A 192.168.1.20
<snip>
```

As you can see the server's IP and DNS entry has changed but ping is still trying the old address. There are multiple possible ways to clear out the cache depending on what you are running.

``` bash
sudo /etc/init.d/nscd restart
sudo /etc/init.d/dnsmasq restart
sudo /etc/init.d/named restart
sudo rndc restart
sudo rndc exec
sudo systemd-resolve --flush-cache
sudo resolvectl flush-caches

ping server.example.com
PING server.example.com(192.168.1.20)
64 bytes from 192.168.1.20: icmp_seq=1 ttl=64 time=0.420 ms
<snip>
```
