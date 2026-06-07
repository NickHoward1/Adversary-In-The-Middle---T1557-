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
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>

<h2>Investigation Process</h2>

Step 1 - 

Filter1:


<img src= "" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

Step 2 - 

Filter1: 

<img src= "" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <img src= "" width="300" height="300"/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

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

