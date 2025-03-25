



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
local=/nokiptchub01.nokia.usa
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