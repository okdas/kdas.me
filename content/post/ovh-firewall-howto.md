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

On OVH.com services, we have an interesting feature that is not currently documented (although I hope it will be soon). It is a Firewall. This feature was launched in 2014 on the brand new managerV6 (control panel), which is now the default.

The Firewall activates automatically during every DDoS attack and it cannot be disabled until the attack is stopped. **That's why it's so important to keep firewall rules up to date!** By default, there are no rules set, so any connections can be established.

A significant characteristic (or bug) is that these rules do not affect connections within the OVH network. I'm confident this isn't a bug because the Firewall is part of their VAC (Anti-DDoS solution) — the Firewall Network on the scheme.

{{< figure src="/media/ovh-firewall-howto/vac-inside.jpg" >}}

The Firewall is incredibly useful in defending against DDoS attacks. For instance, on a regular web server, we only need ports 80 and 443 to be accessible from the internet. We also need rules for outgoing connections (for DNS, mail, system updates, etc.).

Another non-obvious feature is that you can only have 20 rules. Each rule has its own priority. **Higher priority on this table actually means lower priority in the Firewall.** This is a really common mistake.

My best practice is to refuse ALL IPv4 connections and allow only those that I need — usually only 80 and 443. Here is an example:

{{< figure src="/media/ovh-firewall-howto/rules.png" >}}

We have to reject ALL IPv4 traffic on rule 19 (lower priority) and allow ports 80 and 443 for HTTP and HTTPS respectively. We also want to be able to upgrade the system, send emails, etc., so we need outgoing TCP connections to be allowed (rule #13).

For DNS, I'm using OVH's internal nameserver (213.186.33.99), which is available to us because it's inside the OVH network.

That's it, now I'm safe from any DDoS activity, except for HTTP and tcp/syn flood. HTTP flood needs to be filtered at the application layer or on a reverse-proxy (usually, I use Nginx to deal with "bad" IPs). TCP flood is usually well-handled by OVH's VAC.

