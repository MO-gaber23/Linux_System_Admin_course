firewalld

is an service to controll the Network connections with the server as it's an additional layer of security on the Network

- as all atackes is comming from the network so the developers make that additional layer to avoide that attackes
    
- the firewalld is a dynamic firewall manager
    
- the firewalld is used to classify all network traffic into zones
    
- the zone on firewalld is like the profile in the tuned.service as the zone is an some of configrations applied determine the behaviore of the firewalld with the network traffics and services and ports
    
- Each zone has its own list of ports and services that are either open or closed.
    
- as the network communicate with IPs the firewalld classify the trafic into zones Based on criteria such as the source IP address of a packet or the incomming network interface .
    
- the firewalld always checks the source address of evrey packet comming into the system if that IP or network interface is assigend or defined in a specific zone then apply the rules of that zone on that ip or network interface
    

> **NOTE :** if the source IP and the network interface is note assigened or defined in any zone the firewall will apply the ( default Zone ) on them.

- The default zone is not a separate zone, but is a designation for an existing zone.
    
- Initially the firewalld map all network interfaces we have in the default-zone
    
- firewalld maps the loopback interface to the ( Trusted zone )
    
- the trusted zone is the zone which permits all rtaffic by default .
    

> **NOTE :** each zone allow traffics which matche alist of partecular ports and protocols
> 
> - such as port ( 631 ) want to work by the protocol ( udp ) or any service like the pre-defined service such as ( ssh )
>     
> - if the traafic comming does't match a permitted port and protocol or service listed in any zone the firewall will reject that trafice genrally.
>     

# NOTE : the firewalld has a predefined ( zones and services ) which we can use

- we can create an customized zones as we need
    
- we can customize the pre-defined zones too
    
- the firewalld permit all out going traffics by default
    
- all zones permit any incoming trafic which is part of a communication initiated by the system.
    
- the zones allow any incomming trafice related to outgoing traffic we send
    

## ZONE NAME / DEFAULT CONFIGURATION

|Zone Name|Configuration|
|---|---|
|**1- home**|- reject incoming traffic unless the related to out going traffic<br><br>- allow traffic comming from or match the services ( ssh ,mdns ,ipp ,sa,ba-client ,dhcpv6 ) which are pre-defined services|
|**2- internal**|- reject incoming traffic unless the related to out going traffic<br><br>- allow traffic comming from or match the services ( ssh ,mdns ,ipp ,sa,ba-client ,dhcpv6 ) which are pre-defined services|
|**3- work**|- reject incomming traffic unless related to outgoing traffic<br><br>- allow trafices matces ( ssh ,ipp-client ,dhcpv6 ) predefined services|
|**4- public**|Reject incoming traffic unless related to outgoing traffic or matchingthe ssh or dhcpv6 -client pre-defined services. The default zone for newly added network interfaces.|
|**5- external**|Reject incoming traffic unless related to outgoing traffic or matching the ssh pre-defined service. Outgoing IPv4 traffica forwarded through this zone is masqueraded to look like it originated from the IPv4 address of the outgoing network interface.|
|**5- dmz**|Reject incoming traffic unless related to outgoing traffic or matching the ssh pre-defined service.|
|**6- block**|Reject all incoming traffic unless related to outgoing traffic ( respond with ICMP error ).|
|**7- drop**|Drop all incoming traffic unless related to outgoing traffic (do not even respond with ICMP errors ).|

- as we know each network service has its dadecated port that send and recive traffices by it .
    
- so to let the firewalld allow any service to communicate we must configure the service with its dedecated port and protocol
    
- the firewalld provide some pre-defined services with thir dedecated ports and protocole to use without configuring it from the beganing manually
    

# the predefined services is :

## SERVICE NAME / CONFIGURATION

|Service Name|Configuration|
|---|---|
|**1- ssh**|Local SSH server. Traffic to 22/tcp|
|**2- dhcpv6-chient**|Local DHCPv6 client. Traffic to S46/udp on the IPv6 network|
|**3- ipp-client**|Local IPP printing. Traffic to 631/udp.|
|**4- samba-client**|Local Windows file and print sharing client. Traffic to 137/udp and 138/ udp|
|**5- mdns**|Multicast DNS (mDNS) local-link name resolution Traffic to 5353/udp to the 224.00251 (IPv4) or ff02:.fb (IPv6) multicast addressesa.|

> **NOTE :** each service is stored in xml files with its ports and protocoles work with

# to list all predefined services use :

`[ command : firewall-cmd --get-services ]`

```
[root@server ~]# firewall-cmd --get-services

RH-Satellite-6 RH-Satellite-6-capsule afp amanda-client amanda-k5-client amqp amqps apcupsd audit ausweisapp2 bacula bacula-client bareos-director bareos-filedaemon bareos-storage bb bgp bitcoin bitcoin-rpc bitcoin-testnet bitcoin-testnet-rpc bittorrent-lsd ceph ceph-exporter ceph-mon cfengine checkmk-agent cockpit collectd condor-collector cratedb ctdb dds dds-multicast dds-unicast dhcp dhcpv6 dhcpv6-client distcc dns dns-over-tls docker-registry docker-swarm dropbox-lansync elasticsearch etcd-client etcd-server finger foreman foreman-proxy freeipa-4 freeipa-ldap freeipa-ldaps freeipa-replication freeipa-trust ftp galera ganglia-client ganglia-master git gpsd grafana gre high-availability http http3 https ident imap imaps ipfs ipp ipp-client ipsec irc ircs iscsi-target isns jenkins kadmin kdeconnect kerberos kibana klogin kpasswd kprop kshell kube-api kube-apiserver kube-control-plane kube-control-plane-secure kube-controller-manager kube-controller-manager-secure kube-nodeport-services kube-scheduler kube-scheduler-secure kube-worker kubelet kubelet-readonly kubelet-worker ldap ldaps libvirt libvirt-tls lightning-network llmnr llmnr-client llmnr-tcp llmnr-udp managesieve matrix mdns memcache minidlna mongodb mosh mountd mqtt mqtt-tls ms-wbt mssql murmur mysql nbd nebula netbios-ns netdata-dashboard nfs nfs3 nmea-0183 nrpe ntp nut openvpn ovirt-imageio ovirt-storageconsole ovirt-vmconsole plex pmcd pmproxy pmwebapi pmwebapis pop3 pop3s postgresql privoxy prometheus prometheus-node-exporter proxy-dhcp ps2link ps3netsrv ptp pulseaudio puppetmaster quassel radius rdp redis redis-sentinel rpc-bind rquotad rsh rsyncd rtsp salt-master samba samba-client samba-dc sane sip sips slp smtp smtp-submission smtps snmp snmptls snmptls-trap snmptrap spideroak-lansync spotify-sync squid ssdp ssh steam-streaming svdrp svn syncthing syncthing-gui syncthing-relay synergy syslog syslog-tls telnet tentacle tftp tile38 tinc tor-socks transmission-client upnp-client vdsm vnc-server warpinator wbem-http wbem-https wireguard ws-discovery ws-discovery-client ws-discovery-tcp ws-discovery-udp wsman wsmans xdmcp xmpp-bosh xmpp-client xmpp-local xmpp-server zabbix-agent zabbix-server zerotier
```

# Configuring the Firewalld :

we have three ways to configure the firewalld :

1. by editing the configration file in ( `/usr/bin/firewalld/` ) not recommended as it store the default configrations that we mostly use to restore our configrations , or we can edit the files in ( `/etc/firewalld/` ) recommended to edit here as any edit here will superseed the default configrations .
    
2. by graphical interface like the ( cockpit with the web-consol ) or with the graphical tool ( `firewall.config` )
    
3. with command `[ firwall-cmd ]`
    

# firewall-cmd command :

- its installed as part of the main firewalld package
    
- firewall-cmd interacts with the firewalld dynamic firewall manager.
    
- this command ( `firewall-cmd` ) used in scripting the firewall setup.
    

> **NOTE :** command ( `firewall-cmd` ) change the configrations at runtime

> **NOTE :** any shange with ( `firewall-cmd` ) is not permanent

# to make the chnges are permanent use option ( `--permanent` )

[ after any change in the configrations to make effiect in the configrations we must reload the service with ]

`[ command:( firewall-cmd -reload )]`

**all supported options we have with the command :firewall-cmd :**

```
--add-forward
--add-forward-port=
--add-icmp-block=
--add-icmp-block-inversion
--add-interface=
--add-lockdown-whitelist-command=
--add-lockdown-whitelist-context=
--add-lockdown-whitelist-uid=
--add-lockdown-whitelist-user=
--add-masquerade
--add-port=
--add-protocol=
--add-rich-rule=
--add-service=
--add-source=
--add-source-port=
--change-interface=
--change-source=
--check-config
--complete-reload
--direct
--get-active-zones
--get-default-zone
--get-helpers
--get-icmptypes
--get-ipset-types
--get-log-denied
--get-policies
--get-services
--get-zone-of-interface=
--get-zones
--help
--info-helper=
--info-icmptype=
--info-ipset=
--info-policy=
--info-service=
--info-zone=
--list-all
--list-all-policies
--list-all-zones
--list-forward-ports
--list-icmp-blocks
--list-interfaces
--list-lockdown-whitelist-commands
--list-lockdown-whitelist-contexts
--list-lockdown-whitelist-uids
--list-lockdown-whitelist-users
--list-ports
--list-protocols
--list-rich-rules
--list-services
--list-source-ports
--list-sources
--lockdown-off
--lockdown-on
--panic-off
--panic-on
--permanent
--policy=
--query-forward
--query-forward-port=
--query-icmp-block=
--query-icmp-block-inversion
--query-interface=
--query-lockdown
--query-lockdown-whitelist-command=
--query-lockdown-whitelist-context=
--query-lockdown-whitelist-uid=
--query-lockdown-whitelist-user=
--query-masquerade
--query-panic
--query-port=
--query-protocol=
--query-rich-rule=
--query-service=
--query-source=
--query-source-port=
--reload
--remove-forward
--remove-forward-port=
--remove-icmp-block=
--remove-icmp-block-inversion
--remove-interface=
--remove-lockdown-whitelist-command=
--remove-lockdown-whitelist-context=
--remove-lockdown-whitelist-uid=
--remove-lockdown-whitelist-user=
--remove-masquerade
--remove-port=
--remove-protocol=
--remove-rich-rule=
--remove-service=
--remove-source=
--remove-source-port=
--reset-to-defaults
--runtime-to-permanent
--set-default-zone=
--set-log-denied=
--state
--version
--zone=
```

# we can fined all zones and services we have in the dir ( `/usr/lib/firewalld/` )

```
[root@server ~]# ls /usr/lib/firewalld/

helpers  icmptypes  ipsets  policies  services  zones
```

- each has it's own directory
    
- each service and zone configrations and info are in XML-file
    

```
[root@server ~]# ls /usr/lib/firewalld/zones/

block.xml  drop.xml      home.xml      nm-shared.xml  trusted.xml
dmz.xml    external.xml  internal.xml  public.xml     work.xml
```

like the zone public :

```
 GNU nano 5.6.1  /usr/lib/firewalld/zones/public.xml             <?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>For use in public areas. You do not trust the othe>  <service name="ssh"/>
  <service name="dhcpv6-client"/>
  <service name="cockpit"/>
  <forward/>
</zone>

                         [ Read 9 lines ]
^G Help      ^O Write Out ^W Where Is  ^K Cut       ^T Execute
^X Exit      ^R Read File ^\ Replace   ^U Paste     ^J Justify
```

# to get the default zone use option `[ --get-default-zone ]`

```
[root@server ~]# firewall-cmd --get-default-zone

trusted
```

# to set default zone use option `[ --set-default-zone=Zone-name ]`

```
[root@server ~]# firewall-cmd --set-default-zone=work

success

[root@server ~]# firewall-cmd --get-default-zone
work
```

> **NOTE :** setting default zone is pemanently we don't use option `--permanent` to make it permanent

# to list all avilable zone use :

`[ firewall-cmd --get-zones ]`

```
[root@server ~]# firewall-cmd --get-zones

block dmz drop external home internal nm-shared public trusted work
```

# to list the active zones : which are in use by having interface or source tied to them :

`[ firewall-cmd --get-active-zones ]`

```
[root@server ~]# firewall-cmd --get-activ
e-zones
work
  interfaces: ens224
```

- as we see we can activat any zones and make it scane the traffics in the intarface connecting with them.
    

# to list all the configration applied on the default zone use :

`[ command : firewall-cmd --list-all ]`

```
[root@server ~]# firewall-cmd --list-all
work (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens224
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

# to list configrations applied on spacific zone use :

`[ command : firewall-cmd --list-all --zone=zone_name ]`

```
[root@server ~]# firewall-cmd --list-all --zone=external
external
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: ssh
  ports:
  protocols:
  forward: yes
  masquerade: yes
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

# we can also display the config for all zones :

`[ command : firewall-cmd --list-all-zones ]`

```
[root@server ~]# firewall-cmd --list-all-zones
block
  target: %%REJECT%%
  icmp-block-inversion: no
  interfaces:
  sources:
  services:
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

dmz
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

drop
  target: DROP
  icmp-block-inversion: no
  interfaces:
  sources:
  services:
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

external
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: ssh
  ports:
  protocols:
  forward: yes
  masquerade: yes
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

home
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: cockpit dhcpv6-client mdns samba-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

internal
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: cockpit dhcpv6-client mdns samba-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

nm-shared
  target: ACCEPT
  icmp-block-inversion: no
  interfaces:
  sources:
  services: dhcp dns ssh
  ports:
  protocols: icmp ipv6-icmp
  forward: no
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
        rule priority="32767" reject

public
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: cockpit dhcpv6-client nfs ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

trusted
  target: ACCEPT
  icmp-block-inversion: no
  interfaces:
  sources:
  services:
  ports: 3260/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

work (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens224
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

# to list the zone activated with an spacific interface :

`[ command : firewall-cmd --get-zone-of-interface="interface_name" ]`

```
[root@server ~]# firewall-cmd --get-zone-of-interface=ens224

work
```

# to add interface to the default zone :

`[ command : firewall-cmd --add-interface=interface_name ]`

```
[root@server ~]# firewall-cmd --add-interface=ens160

success

[root@server ~]# firewall-cmd --list-all | grep interfac
 
  interfaces: ens160 ens224
```

# to add interface to spacified zone :

`[ command : firewall-cmd --add-interface=interface_name --zone=Zone_name ]`

```
[root@server ~]# firewall-cmd --add-interface=ens160 --zone=dmz

success


[root@server ~]# firewall-cmd --list-all --zone=dmz | grep interfac

  interfaces: ens160
```

# to remove interface :

```
[root@server ~]# firewall-cmd --remove-interface=ens160

success
```

# to see if an interface being added on zone or note :

`[ firewall-cmd --query-interface=interface_name ]`

```
[root@server ~]# firewall-cmd --query-interface=ens224

yes
```

# to see wither an service being added on zone or not :

`[ commmand : firewall-cmd --query-service=service_name ]`

```
[root@server ~]# firewall-cmd --query-service=ssh
yes
```

- note the same thing with ports
    
- we can query all of this :
    

```
--query-forward
--query-forward-port=
--query-icmp-block=
--query-icmp-block-inversion
--query-interface=
--query-lockdown
--query-lockdown-whitelist-command=
--query-lockdown-whitelist-context=
--query-lockdown-whitelist-uid=
--query-lockdown-whitelist-user=
--query-masquerade
--query-panic
--query-port=
--query-protocol=
--query-rich-rule=
--query-service=
--query-source=
--query-source-port=
```

# to list the services of any zone :

`[ command: firewall-cmd --list-service --zone=zone_name ]`

```
[root@server ~]# firewall-cmd --list-services --zone=work

cockpit dhcpv6-client ssh
```

- NOTE : we can list ports services zones so that all we can list :
    

```
--list-all
--list-all-policies
--list-all-zones
--list-forward-ports
--list-icmp-blocks
--list-interfaces
--list-lockdown-whitelist-commands
--list-lockdown-whitelist-contexts
--list-lockdown-whitelist-uids
--list-lockdown-whitelist-users
--list-ports
--list-protocols
--list-rich-rules
--list-services
--list-source-ports
--list-sources
```

# now we will do an exampel :

**1- ping on two different ips of anothe machine from different ips :**

```
[root@server ~]# ping 192.168.233.17 -I 192.168.233.16 -c 3
PING 192.168.233.17 (192.168.233.17) from 192.168.233.16 : 56(84) bytes of data.
64 bytes from 192.168.233.17: icmp_seq=1 ttl=64 time=0.723 ms
64 bytes from 192.168.233.17: icmp_seq=2 ttl=64 time=1.97 ms
64 bytes from 192.168.233.17: icmp_seq=3 ttl=64 time=1.95 ms

--- 192.168.233.17 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 0.723/1.545/1.966/0.581 ms

---


[root@server ~]# ping 192.168.233.131 -I 192.168.233.48 -c 5
PING 192.168.233.131 (192.168.233.131) from 192.168.233.48 : 56(84) bytes of data.
64 bytes from 192.168.233.131: icmp_seq=1 ttl=64 time=0.683 ms
64 bytes from 192.168.233.131: icmp_seq=2 ttl=64 time=0.573 ms
64 bytes from 192.168.233.131: icmp_seq=3 ttl=64 time=2.00 ms
64 bytes from 192.168.233.131: icmp_seq=4 ttl=64 time=0.630 ms
64 bytes from 192.168.233.131: icmp_seq=5 ttl=64 time=1.92 ms
```

**2- go to the server machine and do :**

- add the frist ip of the client machine into block zone
    

```
[root@server ~]# firewall-cmd --add-source=192.168.233.17/32 --zone=block

  success
```

- add the seconed ip of the client machine into drop zone
    

```
[root@server ~]# firewall-cmd --add-source=192.168.233.131 --zone=drop
  success
```

**3- try to ping on the server ips from the client machen**

```
[root@client ~]# ping 192.168.233.16 -I 192.168.233.17 -c 5
PING 192.168.233.16 (192.168.233.16) from 192.168.233.17 : 56(84) bytes of data.
From 192.168.233.16 icmp_seq=1 Packet filtered
From 192.168.233.16 icmp_seq=2 Packet filtered
From 192.168.233.16 icmp_seq=3 Packet filtered
From 192.168.233.16 icmp_seq=4 Packet filtered
From 192.168.233.16 icmp_seq=5 Packet filtered

--- 192.168.233.16 ping statistics ---
5 packets transmitted, 0 received, +5 errors, 100% packet loss, time 4070ms
```

> [ we will see that the server send back with ICMP error so the ip we ping by is listed on the block zone in the server machine firewall ]

```
[root@client ~]# ping 192.168.233.48 -I 192.168.233.131 -c 7
PING 192.168.233.48 (192.168.233.48) from 192.168.233.131 : 56(84) bytes of data.

--- 192.168.233.48 ping statistics ---
7 packets transmitted, 0 received, 100% packet loss, time 6133ms
```

> [ no responed from the server ]

**Example : allow to connect to the web server :**

1- by adding the http seveice to the default zone :

```
[root@server ~]# firewall-cmd --add-service=http --permanent --zone=public

   success


[root@server ~]# firewall-cmd --reload
success
```

- NOTE : if we send an request by curl command it will resbond :
    

```
[root@client ~]# curl http://server/index.html

this is the web server
```

2- we can add the port of this service with it's protocol into the public zone :

```
[root@client ~]# firewall-cmd --add-port=80/tcp --permanent --zone=public

success


[root@client ~]# firewall-cmd --reload
success


[root@client ~]# firewall-cmd --list-all --zone=public
public
  target: default
  icmp-block-inversion: no
  interfaces:
  sources:
  services: cockpit dhcpv6-client nfs ssh
  ports: 80/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

- NOTE : we add to the public zone as it's the default and active one .
    

# to enable service or port/protocol for some time :

[ we use option `--timeout=number_of_seconds` ]

```
[root@client ~]# firewall-cmd --add-port=22/tcp --timeout=15
success
```

- NOTE : this method is used in traupelshooting
    

# to create new service that can be enabeld with firewalld :

- go to the dir ( `/etc/firewalld/services` )
    

```
[root@client ~]# cd /etc/firewalld/services/
```

- touch an xml file with the ( `service_name.xml` )
    

```
[root@client services]# touch myssh.xml
[root@client services]# ls
  myssh.xml
```

- put the configration of the service :
    
- as we know the dir ( `/usr/lib/firewalld/services/` ) contain the avilable services we can enable with the firewall
    
- so we can reuse some configrations from the services xml files located in ( `/usr/lib/firewalld/services/` )
    
- so we copied some config from ssh service and edit it to the new service ( myssh ) to have port "2222"
    

as we see in the config :

```
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>MYSSH</short>
  <description>Secure Shell (SSH) is a protocol for logging into and executing commands on remote machines. It provides secure encrypted communications. If you plan on accessing your machine remotely via SSH over a firewalled interface, enable this option. You need the openssh-server package installed for this option to be useful.</description>
  <port protocol="tcp" port="2222"/>
</service>
```

now we will reload the firewall

if we grep on the new service :

```
[root@server ~]# firewall-cmd --get-services | grep myssh

RH-Satellite-6 RH-Satellite-6-capsule afp amanda-client amanda-k5-client amqp amqps apcupsd audit ausweisapp2 bacula bacula-client bareos-director bareos-filedaemon bareos-storage bb bgp bitcoin bitcoin-rpc bitcoin-testnet bitcoin-testnet-rpc bittorrent-lsd ceph ceph-exporter ceph-mon cfengine checkmk-agent cockpit collectd condor-collector cratedb ctdb dds dds-multicast dds-unicast dhcp dhcpv6 dhcpv6-client distcc dns dns-over-tls docker-registry docker-swarm dropbox-lansync elasticsearch etcd-client etcd-server finger foreman foreman-proxy freeipa-4 freeipa-ldap freeipa-ldaps freeipa-replication freeipa-trust ftp galera ganglia-client ganglia-master git gpsd grafana gre high-availability http http3 https ident imap imaps ipfs ipp ipp-client ipsec irc ircs iscsi-target isns jenkins kadmin kdeconnect kerberos kibana klogin kpasswd kprop kshell kube-api kube-apiserver kube-control-plane kube-control-plane-secure kube-controller-manager kube-controller-manager-secure kube-nodeport-services kube-scheduler kube-scheduler-secure kube-worker kubelet kubelet-readonly kubelet-worker ldap ldaps libvirt libvirt-tls lightning-network llmnr llmnr-client llmnr-tcp llmnr-udp managesieve matrix mdns memcache minidlna mongodb mosh mountd mqtt mqtt-tls ms-wbt mssql murmur mysql myssh nbd nebula netbios-ns netdata-dashboard nfs nfs3 nmea-0183 nrpe ntp nut openvpn ovirt-imageio ovirt-storageconsole ovirt-vmconsole plex pmcd pmproxy pmwebapi pmwebapis pop3 pop3s postgresql privoxy prometheus prometheus-node-exporter proxy-dhcp ps2link ps3netsrv ptp pulseaudio puppetmaster quassel radius rdp redis redis-sentinel rpc-bind rquotad rsh rsyncd rtsp salt-master samba samba-client samba-dc sane sip sips slp smtp smtp-submission smtps snmp snmptls snmptls-trap snmptrap spideroak-lansync spotify-sync squid ssdp ssh steam-streaming svdrp svn syncthing syncthing-gui syncthing-relay synergy syslog syslog-tls telnet tentacle tftp tile38 tinc tor-socks transmission-client upnp-client vdsm vnc-server warpinator wbem-http wbem-https wireguard ws-discovery ws-discovery-client ws-discovery-tcp ws-discovery-udp wsman wsmans xdmcp xmpp-bosh xmpp-client xmpp-local xmpp-server zabbix-agent zabbix-server zerotier
```

# to create new zone :

`[ firewall-cmd --permanent --new-zone=new_zone_name ]`

```
[root@server ~]# firewall-cmd --permanent --new-zone=myz
one
success
```

- NOTE : we must use option `--permanent` to make the new zone
    
- if we list files on ( `/etc/firewalld/zones/` ) we will fined the new zone :
    

```
[root@server ~]# cd /etc/firewalld/zones/
[root@server zones]# ls

myzone.xml  public.xml.old  work.xml
public.xml  trusted.xml
```

- if we open the file of new zone we will fined no configration :
    

`GNU nano 5.6.1 myzone.xml`

```
 <?xml version="1.0" encoding="utf-8"?>
<zone>
# here we will write the configrations

# we can write the services or port of this zone manualy or by the command : firewall-cmd --add-


</zone>
```

- NOTE : if we see the dir ( `/usr/lib/firewalld/zones/` ) we won't fined the file of the new zone here .
    

- now we will try to change the interface form the zone public to the new zone we created :
    

# to change interface frome zone to zone :

`[ command : firewall-cmd --change-interface=ifname --zone=the new zone ]`

```
[root@server zones]# firewall-cmd --permanent --change-interface=ens160 --zone=myzone

  success


[root@server zones]# firewall-cmd --get-active-zones
myzone
  interfaces: ens160
public
  interfaces: ens224
```

- adding service to the new zone :
    

```
[root@server zones]# firewall-cmd --permanent --add-serv
ice=http --zone=myzone ; firewall-cmd --reload
success
success



[root@server zones]# firewall-cmd --list-all --zone=myzone
myzone (active)
  target: default
  icmp-block-inversion: no
  interfaces: ens160
  sources:
  services: http
  ports:
  protocols:
  forward: no
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

- if we see the xml file of myzone we will see it had edited :
    

```
[root@server zones]# cat myzone.xml

<?xml version="1.0" encoding="utf-8"?>
<zone>
  <service name="http"/>
</zone>
```

- NOTE : if we now tried to connect to the server by the ip of the interface listed in the new zone it will connect as the new zone allow ( http ) service not allow ssh service
    

and we can send request to it by the ip of that interface .

```
┌──(medo㉿ENGMohamedGaber)-[~]
└─$ curl  [http://192.168.233.48/index.html](http://192.168.233.48/index.html)

this is the web server
```

# to stop all the traffices comming and going to or from the server use option --panic-on with the firewall-cmd :

- this applied in case of emergency and attack detected . `[ command : firewall-cmd --panic-on ]`
    
- to make trafic work again :
    

`[ command : firewall-cmd --panic-off ]`

# Note

- For situations where the basic syntax of firewalld is not enough, system administrators can also add rich-rules
    
- rich-rules are a more expressive syntax, to write more complex rules.
    
- If even the rich-rules syntax is not enough, system administrators can also use [ Direct Configuration rules ]
    
- Direct configration rule is basically raw ( iptables syntax ) that will be mixed in with the firewalld rules.
    
- as we use the ips in the rules we write on the firewall.
    

# Rich Rules : the firewalld rich rules allow to create and build custom firewall rules by ( exprissive language ) , and this rules are not coverd by basic firewalld syntax.

- Direct rules : allow admins to insert hard-coded rules into zones managed by firewalld
    
- it can be used to manage all of :
    

1. Port forwarding and masquerading rules for that zone
    
2. Logging rules set for the zone
    
3. Deny rules set for the zone
    
4. Allow rules set for the zone
    

The first matched rule wins. IF there is no match, the default is to typically deny.

# we can know the syntax and use of rich , direct rules from the manual pages in :

`[ command: man firewalld.richlanguage ]`

`[ command: man firewalld.direct ]`
