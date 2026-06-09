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

<b>Filter1:</b>`arp`This filter will show requests and replies (who-has and is-at) pointing to both ARP requests and responses. This helps to see and examine results for any abnormal and repeated requests or responses. (Request)<br>
<b>Filter2:</b>`arp.opcode == 1` This shows all the ARP requests captured from different hosts.(Request)<br>
<b>Filter3:</b>`arp.opcode == 2` Forged ARP poisoning typically uses unsolicited is-at replies (gratuitous/unasked replies). These are strong indicators. (Response)<br>
Filter4:</b>`arp.isgratuitous` A suspicious host sends many unsolicited (gratuitous) ARP replies, especially to multiple destinations. Repeated gratuitous ARPs can indicate an attacker maintaining their poison state.(Response)<br> 
<b>Filter5:</b>`arp && arp.src.proto_ipv4 == 192.168.10.1 && eth.src == 02:aa:bb:cc:00:01` This filter helps to examine the ARP traffic associated with the gateway. <br>
<b>Filter6:</b>`arp.opcode ==2 && _ws.col.info contains "192.168.10.1 is at"` This filter has two parts. The first part focuses on the ARP responses and the second part _ws.col.info contains "192.168.10.1 is at" filters on the content shown in the information column. This filters out the ARP responses, which are pointing the Gateway's IP 192.168.10.1 to the MAC address. But if we look closely, we can see that the attacker uses ARP spoofing to tell the IP to its MAC address. We can also use this `filter arp.opcode == 2 && arp.src.proto_ipv4 == 192.168.10.1`, which shows the same result.<br>
<b>Filter7:</b>`arp.opcode == 2 && arp.src.proto_ipv4 == 192.168.10.1 && eth.src == 02:fe[REDACTED]` this filter narrows down on the attacker's MAC address that has associated itself with the Gateway's IP address.<br>
<b>Filter8:</b>`arp.duplicate-address-detected || arp.duplicate-address-frame` This filter checks and confirms for duplicate MAC address mappings to a single IP address. <br>

<img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/f397d5182cd3314824a75400079e77128b274676/Screenshot%202026-06-07%20at%2015.55.31.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/c92646cfab85471fb06148d9510a6b0d6730e7f9/Screenshot%202026-06-07%20at%2016.01.55.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/b89c375dc8e84ba3fe75c2d467b478c7cbc001bf/Screenshot%202026-06-07%20at%2016.23.49.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

<h2>Findings</h2>
From carrying out the investigation and using various filters to look for IoC's, it was evident that some ARP replies were pointing the Gateway's IP to the suspicious MAC address. The frequency of these ARP replies indicated that this is indeed an ARP spoofing. 

<h2>Indicators of Compromise</h2>

<ul>
  <li>Multiple MAC addresses claiming the same IP address</li>
  <li>High number of ARP replies without matching requests ("gratuitous ARP")</li>
  <li>Abnormal ARP Traffic Volume</li>
</ul>

<h2>MITRE ATT&CK Mapping</h2>
T1557 – Adversary-in-the-Middle

<h2>Recommendations</h2>

Escalate incident according to incident response procedures.<br>
Identify attacker IP and MAC addresses.<br>
Remove malicious ARP entries and restore legitimate mappings.<br>
Block malicious DNS infrastructure.<br>
Force password resets for affected users.<br>
Review captured credentials for potential compromise.<br>
Investigate affected endpoints for persistence mechanisms.<br>
Document findings and support recovery efforts.<br>

<h2>Lessons Learned</h2>
This investigation improved my understanding of how attackers can chain multiple Man-in-the-Middle techniques together to intercept, manipulate, and capture sensitive communications within a network. Through packet analysis and log review, I learned how to identify ARP Spoofing by detecting conflicting MAC address mappings, DNS Spoofing through malicious DNS responses.

<h2>Indicators of attack</h2> 

We can look for the following key indicators while investigating the logs or network traffic for a potential Man-in-the-Middle attack using ARP spoofing.

Duplicate MAC-to-IP Mappings: Multiple MAC addresses claiming the same IP address. Indicates impersonation.<br>
Unsolicited ARP Replies: High number of ARP replies without matching requests ("gratuitous ARP").<br>
Abnormal ARP Traffic Volume: A Large number of ARP packets in short intervals.<br>
Unusual Traffic Routing: Traffic rerouted through the attacker’s MAC.<br>
Gateway Redirection Patterns: Multiple destination MACs for the same gateway IP.<br>
ARP Probe / Reply Loops: Many ARP requests with Who has 192.168.1.x? Tell 192.168.1.y patterns.<br>


<h1>Man In the Middle (DNS Spoofing)</h1>

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

Step 1 - Analyse the PCAPs and looks for anomolies in Wireshark

<b>Filter1:</b> Narrowing down DNS traffic `dns` This filter will allow us to inspect requests/replies and notice abnormal volume or patterns. <br>
<b>Filter2:</b> Filtering on Legit traffic `dns.flags.response == 1 && ip.src == 8.8.8.8` Legitimate DNS servers (like Google’s 8.8.8.8) respond from a known external IP address. By filtering responses from this IP, I can see what normal answers look like for comparison.<br>
<b>Filter3:</b> Examine DNS Responses `dns.flags.response==1` In the output, I hunt for responses from IP addresses other than the usual DNS server. Looking closely, I found a DNS response from an IP other than 8.8.8.8. I have discovered an odd DNS request indicating a potential DNS spoofing attempt, I've taken a note and will come back to this.<br>
<b>Filter4:</b> DNS response from DNS Server `dns.flags.response == 1 && ip.src == 8.8.8.8` I found an interesting domain, corp-login.acme-corp.local.<br> 
<b>Filter5:</b> Inspect the DNS traffic for the domain I found `dns && dns.qry.name == "corp-login.acme-corp.local"`<br>
<b>Filter6:</b>  filter on the DNS request for the domain found `dns.flags.response == 1 && ip.src == 8.8.8.8 && dns.qry.name == "corp-login.acme-corp.local"` everything looks normal, see screenshot 4<br>
<b>Filter7:</b> DNS responses other than the DNS Server. `dns.flags.response == 1 && ip.src != 8.8.8.8 && dns.qry.name == "corp-login.acme-corp.local"` The last screenshot show DNS spoofing, It shows a system within the network acts as a rogue DNS server, sending spoofed DNS responses.

<img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/991845a68c22c07a0287321c99a96bf6437e643e/Screenshot%202026-06-09%20at%2019.26.29.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp<img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/725d0ea380412673a34c926dfa18e82bb839dc5a/Screenshot%202026-06-09%20at%2019.31.08.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp<img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/de373767f75085eb5a4d6901450665dcd94fe030/Screenshot%202026-06-09%20at%2019.39.48.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp <img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/c74e6bf14ff51f86fd573f8f5b96b0d7220f1f9f/Screenshot%202026-06-09%20at%2019.47.34.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp <img src= "https://github.com/NickHoward1/Adversary-In-The-Middle---T1557-/blob/322db79a1b4fe7910dca0079d86f6a5d0a51ea32/Screenshot%202026-06-09%20at%2020.12.42.png" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp



<h2>Findings</h2>

Eveything seemed legitimate traffic, until an IP address and odd DNS request, I carried out a filter to see DNS responses other thaan the DNS server, which later on I found DNS spoofing. 

<h2>Indicators of Compromise</h2>

<ul>
  <li></li>
  <li></li>
  <li></li>
</ul>

<h2>MITRE ATT&CK Mapping</h2>
T1557 – Adversary-in-the-Middle
T1557.002 - DNS Spoofing

<h2>Recommendations</h2>
Implement DNSSEC validation, enhance DNS traffic monitoring within the SIEM, deploy IDS signatures for DNS spoofing and ARP poisoning activity, and adopt encrypted DNS protocols where feasible. Regular network security reviews and user awareness training should also be conducted to reduce the likelihood and impact of similar attacks.


<h2>Lessons Learned</h2>
Analysis of the packet capture revealed successful DNS response spoofing, allowing malicious DNS records to be returned before legitimate responses. This demonstrated how attackers can manipulate name resolution processes to redirect users to attacker-controlled resources. The investigation highlighted the importance of DNS monitoring, network segmentation, and secure DNS validation mechanisms.


<h2>Indicators of attack</h2> 

We can look for the following key indicators while investigating the logs or network traffic for a potential Man-in-the-Middle attack using DNS spoofing.<br>

Multiple DNS responses for the same query: A legitimate resolver and a forged responder reply to the same query. This is the single most reliable indicator.<br>
DNS response from an unexpected source: A DNS reply arrives from an IP address that does not match any configured resolver (like 8.8.8.8 or your DNS server).<br>
Suspiciously short TTL (Time-To-Live) values: Attackers use very low TTLs (1 - 30s) to keep poisoned entries short-lived and reassert control.<br>
Unsolicited DNS responses: A DNS reply appears without a corresponding DNS request from the victim.<br>


