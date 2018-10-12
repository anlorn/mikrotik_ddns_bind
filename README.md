# Implementation of Mikrotik script for using DYNAMIC DNS with remote BIND server

Script keeps updated IP for a some specific hostname on remote BIND server. It uses pregenerated key. Right now Mikrotik supports
only "hmac-md5". [How to generate key for BIND and give permsission for update](https://www.cyberciti.biz/faq/unix-linux-bind-named-configuring-tsig/).

As soon as key generated you can test it with `nsupdate` and if everything works, just set configuration valiables in the script. And run script in scheduler.

### Script confiuration valirables:
 - wanInterface - An interface of Mikrotik which IP address you want to use for domain, for example `ether1`
 - masterDnsHost - Hostname of BIND where Dynamic update would be sent, for example `ns1.example.com`
 - hostnameForUpdate - Hostname for which IP should be updated, for example `ddns.example.com`
 - keyName - Name of the key which was generated for the BIND server. Be careful, wrong name will result in a error on update. Name must be exactly the same as defined on BIND server. For example `ddns.example.com`
 - key - key itself generated for the BIND server, for example `MeFWeYDw1PUhMCdMR2fU8w==`
.
If something doesn't work just try to run `nsupdate` on local machine just to be sure that bind configured correct. For examples above you do it this way ` nsupdate -y hmac-md5:ddns.example.com:MeFWeYDw1PUhMCdMR2fU8w==`, then
```server <IP of ns1.example.com>
zone example.com.
add ddns.example.com. 86400 IN A 10.20.30.40
show
send
```
