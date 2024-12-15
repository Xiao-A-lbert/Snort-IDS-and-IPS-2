# Snort IDS and IPS 2

<h2>Description</h2>
In this task, I used wireshark to examine a sample pcap where an endpoint user was infected by a qakbot malware and found IOCs. 


<h2>Languages and Utilities Used</h2>

- <b>Snort</b>
- <b>linux CLI</b>


<h2>Environments Used </h2>

- <b>Unbuntu</b> 

<br />
<br />
First changed time format to standard UTC.

![1) snort rule alert tcp 80 exe traffic ](https://github.com/user-attachments/assets/873cb8a5-9322-4b2e-94df-7c78f9b067cd)

<br />
<br />
Next I looked at Capture File Properties to exammine hashes, length, time of capture, and total packets. 

![2) run snort on pcap1 shows 2 alerts for tcp 80 exe traffic](https://github.com/user-attachments/assets/2fec173b-72f9-4b49-aad9-f5f21a7a8354)

<br />
<br />  
I looked at conversation statistics to look at top ips of communication sorted by packets. Presumably the infected host os ip 10.0.0.149 and is reaching out to malicious 10.0.0.6. Other top 5 ips are also enumerated. 

![3) reading snort log in decode format to show exe GET](https://github.com/user-attachments/assets/18273b50-14d1-4d95-b780-232f2799732f)

<br />
<br />
Looking at Protocol Hierarch Statistics, I can see 4 http apckets, along with 37 smtp packets. 

![4) follow http stream shows content type](https://github.com/user-attachments/assets/b091b203-a295-4fbb-a5c7-552ce731915d)

<br />
<br />
Before diving further, I added src and dst port columns. 

![5) new snort rule alert tcp 80 content type application x msdownload ](https://github.com/user-attachments/assets/225b0b91-24b5-49ef-a709-8bb2bcdf5e86)

<br />
<br />
Filtering for http traffic and following the get request for a .dat file. 

![6) alert](https://github.com/user-attachments/assets/d6370dbc-fd06-4057-9324-1b6d7d4a36ce)

<br />
<br />
This shows the user-agent using a curl command to get a 86607 .dat file. 

![7) decode content type packet alert](https://github.com/user-attachments/assets/273a8403-cb87-4a0e-b62c-f4f5a0b6d0ce)

<br />
<br />  
Similarly, going to statistics>http>requests will show all the http requests which enumerates the 86607 .dat file. 

![8) new snort rule to detect file sigantures incoming 80 traffic](https://github.com/user-attachments/assets/7211bcc8-6a53-42a8-b648-8c03cd0c7628)

<br />
<br />
I saved the file into a directory to show details of 86607 .dat being a PE32 executable and generating a sha256 hash for it. 

![9) rule alert MZ signature](https://github.com/user-attachments/assets/1752b61b-cb3d-4531-a4b3-1dcba878badc)

<br />
<br />
Virustotal shows the file hash to be malicious with 57 vendor flags for qakbot. 

![10) wireshark pcap shows SSLoad UA](https://github.com/user-attachments/assets/522b0865-ed86-4f79-b70b-9bb9f04d01a9)

<br />
<br />
Looking up the hash on malware bazaar also shows a quakbot signature. 

![11) SSLoad snort rule](https://github.com/user-attachments/assets/e9ebb48f-1c02-4a9e-8790-b118c79244bd)

<br />
<br />
Doing some OSINT, cyber.gc.ca & MITRE shows Qakbot observed to perform ARP scans to discover endpoints on the network. 

![12) 9 alerts for SSL in httpheader in 2pcap detected](https://github.com/user-attachments/assets/a24c2d02-2488-455f-8489-e7b09ffde37b)

<br />
<br />  
Filtering for arp and ethernet destination of a broadcast shows 10.0.0.149 performing a descending arp scan. 

![13) 3pcap shows ssh bruteforce](https://github.com/user-attachments/assets/076d989f-ab5e-4b14-b773-361703070ae3)

<br />
<br />
Filtering for icmp, our endpoint victim 10.0.0.149 shows to ping 10.0.0.6 and 10.0.0.1.  

![14) snort rule for ssh brute force](https://github.com/user-attachments/assets/e3ef0049-c155-4011-9399-3d1fe0fdb5e9)

<br />
<br />
Filtering specifically for ip 10.0.0.1 shows failed tcp connections over port 445, 139, and 80.

![15) single ssh alert generated ](https://github.com/user-attachments/assets/259f39bb-d27b-40a8-bced-49af5d979c17)

<br />
<br />
Moving on to smtp traffic, we see 37 packets one of which is a AUTH LOGIN.

![16) change threshold type](https://github.com/user-attachments/assets/793f3b6d-2fe2-4cea-85d5-98d4764b1f80)

<br />
<br />
Following the tcp stream of the AUTH LOGIN packet shows a suername and password when decoded from base 64. Even though the authentication failed, passwords may need to be reset for the user for compromised credentials. 

![17) alert for every single ssh attempt  5 within the 30s period](https://github.com/user-attachments/assets/222acd44-1bb9-4320-9acc-ab67227899d2)

<br />
<br />
