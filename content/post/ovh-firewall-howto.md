+++
date = "2015-09-15"
draft = false
comments = true
share = true
title = "OVH Firewall HowTo"
tags = ['ovh', 'firewall', 'vac', 'ddos', 'anti-ddos']
categories = [
  "OVH",
  "hosting"
]
aliases = [ '/ovh-firewall-howto/' ]
+++

*The article not finished yet..*

On OVH.com services we have an interesting feature, which actually not documented at all (yet, I hope). It is a Firewall. It was launched in 2014 on brand new managerV6 (control panel), which is now default.

Firewall turns on automatically on every DDoS attack and it is not possible to shut it down until attack will be stopped. **That is why so important to keep firewall rules actual!** By default, you have no any rules, so any connections can be established.

Huge feature (or bug): those rules will not affect connections inside OVH network. I'm sure that is not a bug because Firewall is a part of their VAC (Anti-DDoS solution) — Firewall Network on scheme.

{{< figure src="/media/ovh-firewall-howto/vac-inside.jpg" >}}

Firewall is really useful against DDoS attacks. For example, on regular web server we only need 80 and 443 ports to be accessible from the internet. Also we need rules for outgoing connections (for DNS, mail, system updates, etc..).

Another one unobvious feature: you can only have 20 rules. Each rule have its own priority. **Higher priority on this table have actually lower priority in Firewall.** That is the really common mistake.

I have a best practice to refuse ALL IPv4 connections and allow only which I need — usually only 80 and 443. And here is example:

{{< figure src="/media/ovh-firewall-howto/rules.png" >}}

We have to reject ALL IPv4 traffic on rule 19 (lower priority) and allow 80 and 443 ports for HTTP and HTTPS respectively. We also want to be able to upgrade the system send e-mails, etc., so we need outgoing TCP connections to be allowed (rule #13).

For DNS I'm using OVH's internal nameserver (213.186.33.99), which is available for us because it is inside OVH network.

That's it, now I'm safe from any DDoS activity, except http and tcp/syn flood. HTTP flood needs to be filtered on application layer or on a reverse-proxy (usually, I'm using Nginx to handle with "bad" IPs). TCP flood usually handles great by OVH VAC.

More examples and explanations will be here later.
