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

# OVH Firewall: A Comprehensive Guide

## Introduction

OVH.com services offer an intriguing feature that, as of this writing, lacks official documentation: the OVH Firewall. Launched in 2014 alongside the new managerV6 (control panel), this firewall has become an integral part of OVH's security infrastructure.

## Automatic Activation During DDoS Attacks

One of the most crucial aspects of the OVH Firewall is its automatic activation during Distributed Denial of Service (DDoS) attacks. Once activated, the firewall cannot be disabled until the attack subsides. This automatic response mechanism underscores the critical importance of maintaining up-to-date firewall rules. By default, no rules are set, allowing all connections to be established.

## Important Characteristics and Limitations

### Internal Network Exemption
A notable characteristic is that these firewall rules do not affect connections within the OVH network. This behavior is intentional, as the Firewall is part of OVH's VAC (Anti-DDoS solution), specifically the "Firewall Network" component in their architecture.

![VAC Architecture](/media/ovh-firewall-howto/vac-inside.jpg)

### Rule Limit and Priority
Users should be aware of several key limitations:
1. There is a maximum of 20 rules per firewall.
2. Each rule has its own priority, but the priority system is counterintuitive. **Higher numbers in the priority column actually indicate lower priority in the Firewall.**
3. The firewall only blocks traffic from outside the OVH network. Internal OVH traffic can still reach your server on any port.

To illustrate the priority system: if you authorize TCP port 80 on priority 18 and then refuse all connections on priority 19, traffic will still flow on port 80. This is because the higher number (19) actually has a lower priority in the firewall's logic. It's almost as if the priority numbers should be reversed, which can be very confusing for users.

## Best Practices

For optimal security, it's recommended to:
1. Deny ALL IPv4 connections by default.
2. Allow only necessary incoming connections (e.g., ports 80 and 443 for a web server).
3. Configure rules for required outgoing connections (e.g., DNS, email, system updates).

Here's an example of a well-configured ruleset:

![Firewall Rules Example](/media/ovh-firewall-howto/rules.png)

In this configuration:
- Rule 19 (lowest priority) rejects ALL IPv4 traffic.
- Higher priority rules allow specific necessary traffic:
  - Ports 80 and 443 for HTTP and HTTPS
  - Outgoing TCP connections for system updates, email sending, etc. (rule #13)

For DNS, this setup utilizes OVH's internal nameserver (213.186.33.99), which is accessible because it's within the OVH network.

## Additional Security Measures

It's crucial to understand that the OVH Firewall is not sufficient for complete server security. Since it only blocks traffic from outside the OVH network, you must implement additional security measures:

1. Use a server-level firewall (such as UFW - Uncomplicated Firewall) to protect against internal OVH network traffic.
2. Secure sensitive ports like SSH (22) using the server-level firewall and other best practices (key-based authentication, non-standard ports, etc.).
3. Regularly update and patch your server to address potential vulnerabilities.

## Conclusion

The OVH Firewall provides robust protection against DDoS attacks and external threats, but it's just one layer of security. For comprehensive protection:

1. Implement the OVH Firewall rules as described above.
2. Use a server-level firewall to guard against internal network threats.
3. Apply additional security measures for services like HTTP and SSH.

Remember, security is an ongoing process. Regularly review and update your firewall rules and security practices to ensure optimal protection for your OVH-hosted services.
