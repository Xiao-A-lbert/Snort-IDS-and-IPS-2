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
Snort rule to generate an alert for tcp traffic over ort 80  containing a .exe with a content type |2e|exe. 

![1) snort rule alert tcp 80 exe traffic ](https://github.com/user-attachments/assets/873cb8a5-9322-4b2e-94df-7c78f9b067cd)

<br />
<br />
Running the snort rule on 1.pcap alerting directly to console shows two tcp alerts containing executables. 

![2) run snort on pcap1 shows 2 alerts for tcp 80 exe traffic](https://github.com/user-attachments/assets/2fec173b-72f9-4b49-aad9-f5f21a7a8354)

<br />
<br />  
Reading the snort log in quiet mode and -d to decode shows the ASCII string of audiodg.exe.

![3) reading snort log in decode format to show exe GET](https://github.com/user-attachments/assets/18273b50-14d1-4d95-b780-232f2799732f)

<br />
<br />
Looking at the http stream in wireshark shows content-type: applicaiton/x-msdownload

![4) follow http stream shows content type](https://github.com/user-attachments/assets/b091b203-a295-4fbb-a5c7-552ce731915d)

<br />
<br />
Snort rule to alert for tcp 80 connections containing content-type: applicaiton/x-msdownload

![5) new snort rule alert tcp 80 content type application x msdownload ](https://github.com/user-attachments/assets/225b0b91-24b5-49ef-a709-8bb2bcdf5e86)

<br />
<br />
Rule generated 1 alert from 1.pcap

![6) alert](https://github.com/user-attachments/assets/d6370dbc-fd06-4057-9324-1b6d7d4a36ce)

<br />
<br />
Reading the snort log in decode format shows the content-type: applicaiton/x-msdownload

![7) decode content type packet alert](https://github.com/user-attachments/assets/273a8403-cb87-4a0e-b62c-f4f5a0b6d0ce)

<br />
<br />  
Writing a snort rule to alert for MZ file signatures.

![8) new snort rule to detect file sigantures incoming 80 traffic](https://github.com/user-attachments/assets/7211bcc8-6a53-42a8-b648-8c03cd0c7628)

<br />
<br />
One alert was generated from the 1.pcap

![9) rule alert MZ signature](https://github.com/user-attachments/assets/1752b61b-cb3d-4531-a4b3-1dcba878badc)

<br />
<br />
Opening wireshark to show the User-Agent: SSLoad 

![10) wireshark pcap shows SSLoad UA](https://github.com/user-attachments/assets/522b0865-ed86-4f79-b70b-9bb9f04d01a9)

<br />
<br />
Writing a snort rule to generate a tcp alert for USer-Agent: SSLoad/1.1

![11) SSLoad snort rule](https://github.com/user-attachments/assets/e9ebb48f-1c02-4a9e-8790-b118c79244bd)

<br />
<br />
9 alerts generated for SSLoad traffic.

![12) 9 alerts for SSL in httpheader in 2pcap detected](https://github.com/user-attachments/assets/a24c2d02-2488-455f-8489-e7b09ffde37b)

<br />
<br />  
Looking at protocol hierarchy stats in wireshark shows 51% ssh packets.

![13) 3pcap shows ssh bruteforce](https://github.com/user-attachments/assets/076d989f-ab5e-4b14-b773-361703070ae3)

<br />
<br />
Writing a snort rule to generate an alert over tcp port 22 for ssh bruke force if 5 attempts occur within 30 seconds with a threshold type of both. Applies to established connections flowing to the server.

![14) snort rule for ssh brute force](https://github.com/user-attachments/assets/e3ef0049-c155-4011-9399-3d1fe0fdb5e9)

<br />
<br />
1 ssh alert generated for 3.pcap.

![15) single ssh alert generated ](https://github.com/user-attachments/assets/259f39bb-d27b-40a8-bced-49af5d979c17)

<br />
<br />
Changing threshold type from both to threshold.

![16) change threshold type](https://github.com/user-attachments/assets/793f3b6d-2fe2-4cea-85d5-98d4764b1f80)

<br />
<br />
This change shows all of the snort alerts if >5 attempts were made within the 30s timeframe.

![17) alert for every single ssh attempt  5 within the 30s period](https://github.com/user-attachments/assets/222acd44-1bb9-4320-9acc-ab67227899d2)

<br />
<br />
