<h1>Man In the Middle (ARP Spoofing)</h1>

<h2>Objective</h2>

Investigate suspicious network activity identified through routine monitoring to determine whether a Man-in-the-Middle (MITM) attack has occurred within the corporate network. Analyse packet captures and network logs to identify evidence of ARP Spoofing, DNS Spoofing, and SSL Stripping techniques used by an attacker to intercept communications, redirect users to malicious destinations, and capture credentials. Determine the scope of the compromise, identify affected hosts, and recommend appropriate containment and remediation actions.

<h2>Scenario</h2>

A routine network monitoring alert at Acme Corp revealed unusual traffic patterns suggesting a possible Man-in-the-Middle (MITM) attack inside the corporate LAN. Over several days, an attacker quietly intercepted communications, redirected connections, and captured user credentials. Using the provided packet capture and logs, you’ll uncover evidence of three chained MITM techniques. 

ARP Spoofing (network interception).<br>
DNS Spoofing (redirection).<br>
SSL Stripping (credential capture).<br>


<h2>Tools Used</h2>
<ul>
  <li>Wireshark</li>
  <li>PCAPs for analysis</li>
</ul>

<h2>Investigation Process</h2>

Step 1 - Use Wireshark to look for IoC's for potential ARP spoofing

Filter1:`arp`This filter will show requests and replies (who-has and is-at) pointing to both ARP requests and responses. This helps to see and examine results for any abnormal and repeated requests or responses. (Request)
Filter2:`arp.opcode == 1` This shows all the ARP requests captured from different hosts.(Request)
Filter3:`arp.opcode == 2` Forged ARP poisoning typically uses unsolicited is-at replies (gratuitous/unasked replies). These are strong indicators. (Response)
Filter4:`arp.isgratuitous` A suspicious host sends many unsolicited (gratuitous) ARP replies, especially to multiple destinations. Repeated gratuitous ARPs can indicate an attacker maintaining their poison state.(Response) 
Filter5:`arp && arp.src.proto_ipv4 == 192.168.10.1 && eth.src == 02:aa:bb:cc:00:01` This filter helps to examine the ARP traffic associated with the gateway. 
Filter6:``arp.opcode ==2 && _ws.col.info contains "192.168.10.1 is at"` This filter has two parts. The first part focuses on the ARP responses and the second part _ws.col.info contains "192.168.10.1 is at" filters on the content shown in the information column. This filters out the ARP responses, which are pointing the Gateway's IP 192.168.10.1 to the MAC address. But if we look closely, we can see that the attacker uses ARP spoofing to tell the IP to its MAC address. We can also use this `filter arp.opcode == 2 && arp.src.proto_ipv4 == 192.168.10.1`, which shows the same result.
Filter7:`arp.opcode == 2 && arp.src.proto_ipv4 == 192.168.10.1 && eth.src == 02:fe[REDACTED]` this filter narrows down on the attacker's MAC address that has associated itself with the Gateway's IP address.
Filter8:`arp.duplicate-address-detected || arp.duplicate-address-frame` This filter checks and confirms for duplicate MAC address mappings to a single IP address. 



<img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/f397d5182cd3314824a75400079e77128b274676/Screenshot%202026-06-07%20at%2015.55.31.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/c92646cfab85471fb06148d9510a6b0d6730e7f9/Screenshot%202026-06-07%20at%2016.01.55.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/b89c375dc8e84ba3fe75c2d467b478c7cbc001bf/Screenshot%202026-06-07%20at%2016.23.49.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

<h2>Findings</h2>


<h2>Indicators of Compromise</h2>

<ul>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>

<h2>MITRE ATT&CK Mapping</h2>


<h2>Recommendations</h2>


<h2>Lessons Learned</h2>

<h2>Indicators of attack</h2> 

We can look for the following key indicators while investigating the logs or network traffic for a potential Man-in-the-Middle attack using ARP spoofing.

Duplicate MAC-to-IP Mappings: Multiple MAC addresses claiming the same IP address. Indicates impersonation.<br>
Unsolicited ARP Replies: High number of ARP replies without matching requests ("gratuitous ARP").<br>
Abnormal ARP Traffic Volume: A Large number of ARP packets in short intervals.<br>
Unusual Traffic Routing: Traffic rerouted through the attacker’s MAC.<br>
Gateway Redirection Patterns: Multiple destination MACs for the same gateway IP.<br>
ARP Probe / Reply Loops: Many ARP requests with Who has 192.168.1.x? Tell 192.168.1.y patterns.<br>

