# B2-R-seauxTP1
      
## 1.Gather informations


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
 
cette route est vers le réseau 10.0.3.15, elle est utilisée pour une connexion locale, la passerelle de cette route est à l'IP 10.0.3.0.(ligne2)
cette route est vers le réseau 192.168.48.4, elle est utilisée pour une connexion externe, la passerelle de cette route est à l'IP 192.168.48.0.(ligne3)


et sa table ARP :

      [root@localhost ~]# ip n
      192.168.48.1 dev enp0s3 lladdr 0a:00:27:00:00:0c DELAY
      10.0.3.2 dev enp0s8 lladdr 52:54:00:12:35:02 REACHABLE


