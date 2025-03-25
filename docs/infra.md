



#### Install the DNS server 

```[root@ncputility ~]# dnf install dnsmasq
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use "rhc" or "subscription-manager" to register.

Last metadata expiration check: 0:27:09 ago on Tue 25 Mar 2025 05:51:40 PM EDT.
Package dnsmasq-2.85-16.el9_4.x86_64 is already installed.
Dependencies resolved.
Nothing to do.
Complete!
[root@ncputility ~]# 
```

##### Configure the DNS

Update this file `/etc/dnsmasq.con` with interface and other information

```

[root@ncputility ~]# cat /etc/dnsmasq.conf|tail -15
#dhcp-name-match=set:wpad-ignore,wpad
#dhcp-ignore-names=tag:wpad-ignore
domain=nokiptchub01.nokia.usa 
local=/nokiptchub01.nokia.usa/
address=/ncputility.nokiptchub01.nokia.usa/135.104.149.142
address=/.apps.nokiptchub01.nokia.usa/135.104.149.143
address=/master-0.nokiptchub01.nokia.usa/135.104.149.144
address=/master-1.nokiptchub01.nokia.usa/135.104.149.145
address=/master-2.nokiptchub01.nokia.usa/135.104.149.148
ptr-record=142.149.104.135.in-addr.arpa.,"ncputility.nokiptchub01.nokia.usa"
ptr-record=143.149.104.135.in-addr.arpa.,"apps.nokiptchub01.nokia.usa"
ptr-record=144.149.104.135.in-addr.arpa.,"master-0.nokiptchub01.nokia.usa"
ptr-record=145.149.104.135.in-addr.arpa.,"master-1.nokiptchub01.nokia.usa"
ptr-record=148.149.104.135.in-addr.arpa.,"master-2.nokiptchub01.nokia.usa"
[root@ncputility ~]# 
```


Start the server here 

```
[root@ncputility ~]# systemctl enable dnsmasq.service; systemctl start dnsmasq.service
[root@ncputility ~]# 
```

Add port to the firewall:

```
[root@ncputility ~]#  sudo firewall-cmd --zone=public --add-port=53/tcp --permanent ; sudo firewall-cmd --zone=public --add-port=53/udp --permanent ; sudo firewall-cmd --reload; sudo firewall-cmd --list-all
success
success
success
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: 140 eno49 eno50 ens1f0 ens1f1
  sources: 
  services: cockpit dhcpv6-client ssh vnc-server
  ports: 5902/tcp 53/tcp 53/udp
  protocols: 
  forward: yes
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
[root@ncputility ~]# 

```


Reload NetworkManager


```
[root@ncputility ~]# echo "[main]" > /etc/NetworkManager/conf.d/90-dns-none.conf; echo "dns=none" >> /etc/NetworkManager/conf.d/90-dns-none.conf
[root@ncputility ~]# systemctl reload NetworkManager
[root@ncputility ~]# 
```

Add sleep to the dnsmasq service, `ExecStartPre=/bin/sleep 30`

```

[root@ncputility ~]# vi /usr/lib/systemd/system/dnsmasq.service
[root@ncputility ~]# cat /usr/lib/systemd/system/dnsmasq.service
[Unit]
Description=DNS caching server.
After=network.target

[Service]
ExecStartPre=/bin/sleep 30
ExecStart=/usr/sbin/dnsmasq
Type=forking
PIDFile=/run/dnsmasq.pid

[Install]
WantedBy=multi-user.target
[root@ncputility ~]# 
```

reload the daemon here 

```
[root@ncputility ~]# sudo systemctl daemon-reload
[root@ncputility ~]# 
```


VAlidate the DNS is working fine here
```
[root@ncputility ~]# nslookup 135.104.149.142
142.149.104.135.in-addr.arpa	name = ncputility.nokiptchub01.nokia.usa.

[root@ncputility ~]# nslookup ncputility.nokiptchub01.nokia.usa
Server:		135.104.149.142
Address:	135.104.149.142#53

Name:	ncputility.nokiptchub01.nokia.usa
Address: 135.104.149.142

[root@ncputility ~]# nslookup raj.apps.nokiptchub01.nokia.usa
Server:		135.104.149.142
Address:	135.104.149.142#53

Name:	raj.apps.nokiptchub01.nokia.usa
Address: 135.104.149.143

[root@ncputility ~]# 
```