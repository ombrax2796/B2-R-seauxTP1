# B2-R-seauxTP1
      
## I.Gather informations


🌞 récupérer une liste des cartes réseau avec leur nom, leur IP et leur adresse MAC
   
   
    [root@localhost ~]#sudo nmcli -f networkmanager
     enp0s8: connected to enp0s8
              "Intel 82540EM"
              ethernet (e1000), 08:00:27:37:F7:76, hw, mtu 1500
              ip4 default
              inet4 10.0.3.15/24
              route4 0.0.0.0/0
              route4 10.0.3.0/24
              inet6 fe80::2c5b:ead:4d52:f0d4/64
              route6 fe80::/64
              route6 ff00::/8

      enp0s3: connected to enp0s3
              "Intel 82540EM"
              ethernet (e1000), 08:00:27:84:EC:E1, hw, mtu 1500
              inet4 192.168.48.4/24
              route4 192.168.48.0/24
              inet6 fe80::a00:27ff:fe84:ece1/64
              route6 ff00::/8
              route6 fe80::/64
   *-------------------
   
   
 🌞 déterminer si les cartes réseaux ont récupéré une IP en DHCP ou non  
          
 sudo nmcli -f DHCP4 con show enp0s3 rien
 sudo nmcli -f DHCP4 con show enp0s8 :
 
 
      DHCP4.OPTION[1]:                        domain_name = auvence.co
      DHCP4.OPTION[2]:                        domain_name_servers = 10.33.10.20 10.33.10.2 8.8.8.8 8.8.4.4
      DHCP4.OPTION[3]:                        expiry = 1569599339
      DHCP4.OPTION[4]:                        ip_address = 10.0.3.15
      DHCP4.OPTION[5]:                        requested_broadcast_address = 1
      DHCP4.OPTION[6]:                        requested_dhcp_server_identifier = 1
      DHCP4.OPTION[7]:                        requested_domain_name = 1
      DHCP4.OPTION[8]:                        requested_domain_name_servers = 1
      DHCP4.OPTION[9]:                        requested_domain_search = 1
      DHCP4.OPTION[10]:                       requested_host_name = 1
      DHCP4.OPTION[11]:                       requested_interface_mtu = 1
      DHCP4.OPTION[12]:                       requested_ms_classless_static_routes = 1
      DHCP4.OPTION[13]:                       requested_nis_domain = 1
      DHCP4.OPTION[14]:                       requested_nis_servers = 1
      DHCP4.OPTION[15]:                       requested_ntp_servers = 1
      DHCP4.OPTION[16]:                       requested_rfc3442_classless_static_routes = 1
      DHCP4.OPTION[17]:                       requested_routers = 1
      DHCP4.OPTION[18]:                       requested_static_routes = 1
      DHCP4.OPTION[19]:                       requested_subnet_mask = 1
      DHCP4.OPTION[20]:                       requested_time_offset = 1
      DHCP4.OPTION[21]:                       requested_wpad = 1
      DHCP4.OPTION[22]:                       routers = 10.0.3.2
      DHCP4.OPTION[23]:                       subnet_mask = 255.255.255.0



🌞 afficher la table de routage de la machine:
      
      [root@localhost ~]# ip route
      default via 10.0.3.2 dev enp0s8 proto dhcp metric 101
      10.0.3.0/24 dev enp0s8 proto kernel scope link src 10.0.3.15 metric 101
      192.168.48.0/24 dev enp0s3 proto kernel scope link src 192.168.48.4 metric 100
 
cette route est vers le réseau 10.0.3.15, elle est utilisée pour une connexion externe, la passerelle de cette route est à l'IP 10.0.3.0.(ligne2)

cette route est vers le réseau 192.168.48.4, elle est utilisée pour une connexion locale, la passerelle de cette route est à l'IP 192.168.48.0.(ligne3)


et sa table ARP :

      [root@localhost ~]# ip n
      192.168.48.1 dev enp0s3 lladdr 0a:00:27:00:00:0c DELAY
      10.0.3.2 dev enp0s8 lladdr 52:54:00:12:35:02 REACHABLE

🌞 récupérer la liste des ports en écoute sur la machine 

      [root@localhost ~]# ss -nutlp
      Netid State   Recv-Q   Send-Q         Local Address:Port     Peer Address:Port
      udp   UNCONN  0        0                  127.0.0.1:323           0.0.0.0:*      users:(("chronyd",pid=743,fd=6))
      udp   UNCONN  0        0           10.0.3.15%enp0s8:68            0.0.0.0:*      users:(("NetworkManager",pid=774,fd=22))
      udp   UNCONN  0        0                      [::1]:323              [::]:*      users:(("chronyd",pid=743,fd=7))
      tcp   LISTEN  0        128                  0.0.0.0:22            0.0.0.0:*      users:(("sshd",pid=784,fd=6))
      tcp   LISTEN  0        128                     [::]:22               [::]:*      users:(("sshd",pid=784,fd=8))

Donc le ports en écoute sur la machine est le service ssh : 128

🌞 récupérer la liste des DNS utilisés par la machine
     
 J'ai du installé dig pour pouvoir faire la suite et faire ceci :
     
      [root@localhost ~]# dig www.reddit.com
      ; <<>> DiG 9.11.4-P2-RedHat-9.11.4-17.P2.el8_0.1 <<>> www.reddit.com
      ;; global options: +cmd
      ;; Got answer:
      ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 21254
      ;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

      ;; OPT PSEUDOSECTION:
      ; EDNS: version: 0, flags:; udp: 1460
      ; COOKIE: b42340a95ff5cea68883a5755d94c446fcf0ce8338bc1147 (good)
      ;; QUESTION SECTION:
      ;www.reddit.com.                        IN      A

      ;; ANSWER SECTION:
      www.reddit.com.         63      IN      CNAME   reddit.map.fastly.net.
      reddit.map.fastly.net.  15      IN      A       151.101.121.140

      ;; Query time: 36 msec
      ;; SERVER: 192.168.1.1#53(192.168.1.1)
      ;; WHEN: Wed Oct 02 11:37:42 EDT 2019
      ;; MSG SIZE  rcvd: 122
      
Les adresses ip sont donc 151.101.121.140 et 192.168.1.1

🌞 afficher l'état actuel du firewall

            [root@localhost ~]# firewall-cmd --list-all
            public (active)
              target: default
              icmp-block-inversion: no
              interfaces: enp0s3 enp0s8
              sources:
              services: cockpit dhcpv6-client ssh
              ports:
              protocols:
              masquerade: no
              forward-ports:
              source-ports:
              icmp-blocks:
              rich rules:

On peut voir que les interfaces emp0s3 et enp0s8 sont filtrées, aucun port n'est autorisés.

## II. Edit configuration

🌞 modifier la configuration de la carte réseau privée

Pour changer l'ip j'ai utilisé nmtui 192.168.48.7
![capture](https://github.com/ombrax2796/B2-R-seauxTP1/blob/master/Capture.PNG "capture")

ajouter une nouvelle carte réseau dans un DEUXIEME réseau privé UNIQUEMENT privé


      [root@localhost ~]# ip a
      1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
          link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
          inet 127.0.0.1/8 scope host lo
             valid_lft forever preferred_lft forever
          inet6 ::1/128 scope host
             valid_lft forever preferred_lft forever
      2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
          link/ether 08:00:27:84:ec:e1 brd ff:ff:ff:ff:ff:ff
          inet 192.168.48.4/24 brd 192.168.48.255 scope global noprefixroute enp0s3
             valid_lft forever preferred_lft forever
          inet6 fe80::a00:27ff:fe84:ece1/64 scope link
             valid_lft forever preferred_lft forever
      3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
          link/ether 08:00:27:37:f7:76 brd ff:ff:ff:ff:ff:ff
          inet 10.0.3.15/24 brd 10.0.3.255 scope global dynamic noprefixroute enp0s8
             valid_lft 78752sec preferred_lft 78752sec
          inet6 fe80::2c5b:ead:4d52:f0d4/64 scope link noprefixroute
             valid_lft forever preferred_lft forever
      4: enp0s9: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
          link/ether 08:00:27:9f:1a:64 brd ff:ff:ff:ff:ff:ff
          inet 192.168.47.5/24 brd 192.168.155.255 scope global noprefixroute enp0s9
             valid_lft forever preferred_lft forever
          inet6 fe80::b824:c276:f895:8b46/64 scope link noprefixroute
             valid_lft forever preferred_lft forever
             
On vérifie les table :

      [root@localhost ~]# ip route
      default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
      10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
      192.168.47.0/24 dev enp0s9 proto kernel scope link src 192.168.47.5 metric 104
      192.168.176.0/24 dev enp0s8 proto kernel scope link src 192.168.176.9 metric 103
      
## 2. Serveur SSH

🌞 modifier la configuration du système pour que le serveur SSH tourne sur le port 2222

Il faut seulement modifier /etc/ssh/sshd_config pour changer de port. Pour cela faire "cd /etc/ssh" et modifier grâce a "vi sshd_config"
  
  
      [root@localhost ~]$ sudo firewall-cmd --permanent --add-port=2222/tcp
      success
      [root@localhost ~]$ sudo firewall-cmd --permanent --remove-port=22/tcp
      success
      [root@localhost ~]$ sudo firewall-cmd --reload
      success
      
## IV. Autres applications et métrologie

🌞 jouer avec iftop
Elle permet de surveiller notre utilisation de la bande passante.

🌞 mettre en place cockpit sur la VM1

      [root@clone ~]$ sudo ss -ltu
      Netid   State     Recv-Q    Send-Q            Local Address:Port                  Peer Address:Port    
      udp     UNCONN    0         0              10.0.2.15%enp0s3:bootpc                     0.0.0.0:*       
      tcp     LISTEN    0         128                     0.0.0.0:EtherNet/IP-1              0.0.0.0:*       
      tcp     LISTEN    0         128                           *:websm                            *:*       
      tcp     LISTEN    0         128                        [::]:EtherNet/IP-1                 [::]:*       
      
🌞 explorer Cockpit, plus spécifiquement ce qui est en rapport avec le réseau
On peut se connecter à l'interface web de cockpit à l'adresse du clone sur le port 9090.
