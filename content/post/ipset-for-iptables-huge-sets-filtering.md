+++
comments = true
date = "2015-08-10"
draft = false
image = ""
menu = ""
share = true
tags = ["iptables"]
title = "ipset for iptables huge sets filtering"
aliases = [ '/ipset-for-iptables-huge-sets-filtering/' ]
+++

Start by installing the necessary software:
`apt-get install zip unzip ipset`

Next, create a hash set:
`ipset create hash_block hash:net`

To block TCP connections, use the following command:

`iptables -I INPUT -p tcp --dport 25565 -m set --match-set sfs_block src -j DROP`


This might be useful with proxy lists (a similar mechanism works on flyspring.net), for example, with the StopForumSpam list:

```
ipset destroy temporably
ipset create hash_block hash:net
ipset create temporably hash:net
wget -N http://flyspring.net/files/listed_ip_7.zip -P /tmp
unzip /tmp/listed_ip_7.zip -d /tmp/
sed 's:^:add tempset :' /tmp/listed_ip_7.txt > /tmp/listed_ip_7_importfile.txt
ipset restore < /tmp/listed_ip_7_importfile.txt
rm /tmp/listed_ip_7_importfile.txt /tmp/listed_ip_7.txt /tmp/listed_ip_7.zip
ipset swap tempset hash_block
```
