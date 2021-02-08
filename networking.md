# Networking tools

The typical tools are:

ifconfig 

``` 
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

```
netstat -ant
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
tcp        0    216 192.168.10.18:22        192.168.10.52:47152     ESTABLISHED
tcp6       0      0 :::22                   :::*                    LISTEN  

```
netstat (-a all, -n no dns resolution, -u udp ports)
```
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
```
sudo netstat -antp
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      641/sshd            
tcp        0    196 192.168.10.18:22        192.168.10.52:47152     ESTABLISHED 28098/sshd: pbultee 
tcp6       0      0 :::22                   :::*                    LISTEN      641/sshd            

```

But these are being replaced with different ones

ip addr show
ss


