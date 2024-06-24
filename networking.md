# Network troubleshooting tools

The typical tools are:

- ifconfig
- netstat
- traceroute

but these are being replaced with newever versions

- ip
- ss

and you can use other tools like telnet or redirects into /dev (more advanced, but pretty useful)

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

== Port testing

If you're testing if a port is open, a common tool to use is `telnet`, however, this tool might not always be available.

```bash
telnet 127.0.0.1 22
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.7
^C
Connection closed by foreign host.
```

But if telnet isn't available, you can use the `/dev` filesystem to get to it.

If you notice below, that path doesn't actually exist. Additionally, you can get to a system via FQDN or IP.

```bash
ls /dev/tcp/
ls: cannot access '/dev/tcp/': No such file or directory
exec 3<>/dev/tcp/127.0.0.1/22
exec 3<>/dev/tcp/127.0.0.1/23
bash: connect: Connection refused
bash: /dev/tcp/127.0.0.1/23: Connection refused
exec 3<>/dev/tcp/www.google.com/80
```

## openssl s_client

You can use `openssl` to test a connection to a TLS endpoint and even see the certificate.

```bash
openssl s_client -connect www.google.com:443 -servername www.google.com -status
CONNECTED(00000004)
depth=2 C = US, O = Google Trust Services LLC, CN = GTS Root R1
verify return:1
depth=1 C = US, O = Google Trust Services, CN = WR2
verify return:1
depth=0 CN = www.google.com
verify return:1
OCSP response: no response sent
---
Certificate chain
 0 s:CN = www.google.com
   i:C = US, O = Google Trust Services, CN = WR2
   a:PKEY: id-ecPublicKey, 256 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jun  3 07:35:32 2024 GMT; NotAfter: Aug 26 07:35:31 2024 GMT
 1 s:C = US, O = Google Trust Services, CN = WR2
   i:C = US, O = Google Trust Services LLC, CN = GTS Root R1
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Dec 13 09:00:00 2023 GMT; NotAfter: Feb 20 14:00:00 2029 GMT
 2 s:C = US, O = Google Trust Services LLC, CN = GTS Root R1
   i:C = BE, O = GlobalSign nv-sa, OU = Root CA, CN = GlobalSign Root CA
   a:PKEY: rsaEncryption, 4096 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jun 19 00:00:42 2020 GMT; NotAfter: Jan 28 00:00:42 2028 GMT
---
Server certificate
-----BEGIN CERTIFICATE-----
MIIEVjCCAz6gAwIBAgIQSXxNwQWIT5YSZuWFnlQXRjANBgkqhkiG9w0BAQsFADA7
MQswCQYDVQQGEwJVUzEeMBwGA1UEChMVR29vZ2xlIFRydXN0IFNlcnZpY2VzMQww
CgYDVQQDEwNXUjIwHhcNMjQwNjAzMDczNTMyWhcNMjQwODI2MDczNTMxWjAZMRcw
FQYDVQQDEw53d3cuZ29vZ2xlLmNvbTBZMBMGByqGSM49AgEGCCqGSM49AwEHA0IA
BDTR6QX1jy6WONOixzdIu+rpEB0vcf+UwqCTbvCddyDYlZQ5QJ2NJfUhB4NMRHW8
Waqb9Z0/Fx62R/zWvLXadsujggJBMIICPTAOBgNVHQ8BAf8EBAMCB4AwEwYDVR0l
BAwwCgYIKwYBBQUHAwEwDAYDVR0TAQH/BAIwADAdBgNVHQ4EFgQU36O8GACznum9
YV13sSejVDUNVEowHwYDVR0jBBgwFoAU3hse7XkV1D43JMMhu+w0OW1CsjAwWAYI
KwYBBQUHAQEETDBKMCEGCCsGAQUFBzABhhVodHRwOi8vby5wa2kuZ29vZy93cjIw
JQYIKwYBBQUHMAKGGWh0dHA6Ly9pLnBraS5nb29nL3dyMi5jcnQwGQYDVR0RBBIw
EIIOd3d3Lmdvb2dsZS5jb20wEwYDVR0gBAwwCjAIBgZngQwBAgEwNgYDVR0fBC8w
LTAroCmgJ4YlaHR0cDovL2MucGtpLmdvb2cvd3IyL29RNm55cjhGMG0wLmNybDCC
AQQGCisGAQQB1nkCBAIEgfUEgfIA8AB2AO7N0GTV2xrOxVy3nbTNE6Iyh0Z8vOze
w1FIWUZxH7WbAAABj909TqUAAAQDAEcwRQIgHJS2fR8YH/2fxkHnTVnoljqN1YSx
UteYb3uIfPCTU4kCIQDPc4mPepJkx2EtSDvz98SMIi1d2RGqJdoOwoHbcTTUVAB2
AD8XS0/XIkdYlB1lHIS+DRLtkDd/H4Vq68G/KIXs+GRuAAABj909TrsAAAQDAEcw
RQIgE7LU0q96nQ7j6PnD1950ptkxsxSVz5R9Cc/BMyocBhoCIQCQd0I5sACFezN2
VhXIM7yNIy/76nDzLl04YUflxb1JmzANBgkqhkiG9w0BAQsFAAOCAQEAYGTkcGY9
iTp1GrXkjeECwQxJPj/yxKEUDr/Yg/ltbqgBePF5gT49WyLEo7pnaB2FqkNtu58x
QO+qsUgFJLJxU/oEK133sJMY32mbgp0S6XJGrIez4PyLh62yhG/f75xXGn8VL3zV
kQaElcaYvzG750goDQyVMkorTIrTxTCWmRfFlHzGCCh5T3KhC7AfCuL169+KKb1C
r+Vty3PwuhmgoPw9oZoYRZnmOUnYXX+YDbW+qmrxSieBbuHEH6y6AZV1IQmgaG2T
0d7nPg0Ed+yMko24fNp+aQEsXfqu6qmVA9ahfXA1BoCnVEy6RocyBmUWgQYf4nl5
vhuLQy1D3JRD9Q==
-----END CERTIFICATE-----
subject=CN = www.google.com
issuer=C = US, O = Google Trust Services, CN = WR2
---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: ECDSA
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 4106 bytes and written 405 bytes
Verification: OK
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 256 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
---

```
