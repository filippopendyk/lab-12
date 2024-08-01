## DNS + DNS Packets

### Objectives

- An online nslookup tutorial can be found and should be read before doing the lab at **`https://www.thegeekstuff.com/2012/07/nslookup-examples/`**
- In this lab, you perform some packet sniffing on DNS packets and you use the command line utility **`nslookup`** to simulate recursive queries. This lab borrows ideas of labs from **`http://en.wikiversity.org`**
- Note that your computer must be connected to the Internet to do this lab.

### Task 1 - Using nslookup

Start a terminal window on the Pi.

```
nslookup google.ca
```

**1. What are the IP address listed for `google.ca`?**

```
172.217.13.131
```

```
nslookup -type=ns google.com
```

**2. What are the name servers listed for this domain name?**

```
google.com      nameserver = ns4.google.com
google.com      nameserver = ns2.google.com
google.com      nameserver = ns1.google.com
google.com      nameserver = ns3.google.com
```

```
nslookup -type=soa google.com
```

**3. What is the start of authority information listed for this zone?**

```
google.com
        primary name server = ns1.google.com
        responsible mail addr = dns-admin.google.com
        serial  = 657515665
        refresh = 900 (15 mins)
        retry   = 900 (15 mins)
        expire  = 1800 (30 mins)
        default TTL = 60 (1 min)
```

```
nslookup -type=mx google.com
```

**4. What are the mail exchangers listed for this domain name?**

```
google.com      MX preference = 10, mail exchanger = smtp.google.com
```



### Task 2 - Domain algonquincollege.com

Redo Task 1 on domain **`algonquincollege.com`**

**5. What are the IP address listed for `algonquincollege.com`?**

```
54.86.119.60
```

**6. What are the name servers listed for this domain name?**

```
algonquincollege.com    nameserver = ns5.algonquincollege.com
algonquincollege.com    nameserver = ns3.algonquincollege.com
```

**7. What is the start of authority information listed for this zone?**

```
algonquincollege.com
        primary name server = ns5.algonquincollege.com
        responsible mail addr = fwadm.algonquincollege.com
        serial  = 2024072904
        refresh = 3600 (1 hour)
        retry   = 3600 (1 hour)
        expire  = 1209600 (14 days)
        default TTL = 3600 (1 hour)
```

**8. What are the mail exchangers listed for this domain name?**

```
algonquincollege.com    MX preference = 10, mail exchanger = algonquincollege-com.mail.protection.outlook.com
```



### Task 3 - Recursive query

Open a terminal window

```
nslookup algonquincollege.com
```

**9. What is the IP address listed?**

```
54.86.119.60
```

To simulate a recursive query:

```
nslookup -norecurse -type=ns com. a.root-servers.net
```

The **`-norecurse`** option forces **`nslookup`** to issue a non-recursive or iterative query. This is the type of query that DNS servers typically issue to other DNS servers.

Observe the results. Notice the name servers listed for the **`com`** domain.

**10. What is the first `com` name server IP address returned?**

```
l.gtld-servers.net      internet address = 192.41.162.30
```



```
nslookup -norecurse -type=ns algonquincollege.com <com. name server>
```

where **`<com. name server>`** is the **`com`** name server IP address listed .

Observe the results.

**11. What are the DNS servers listed for the `algonquincollege.com` domain.**

```
algonquincollege.com    nameserver = ns3.algonquincollege.com
algonquincollege.com    nameserver = ns5.algonquincollege.com
algonquincollege.com    nameserver = ns1.d-zone.ca
algonquincollege.com    nameserver = ns2.d-zone.ca
ns3.algonquincollege.com        internet address = 205.211.50.11
ns5.algonquincollege.com        internet address = 205.211.50.8
```

Select the first **`algonquincollege.com`** name server IP address returned.

```
nslookup -norecurse www.algonquincollege.com <algonquincollege.com. name server>
```

where **`<algonquincollege.com. name server>`** is the first **`algonquincollege.com`** name server IP address listed.

Observe the results.

**12. What is the IP returned?**

```
54.86.119.60
```

**13. Is it the same IP as the IP returned question 9?**

```
yes
```

### Task 4- Packet sniffing

This step is to be done **on your laptop**.

1. Start **`Wireshark`**. (Install it first if needed. Use Google if needed)
2. Start a new capture on your live interface card.
3. Start a wireshark capture by clicking on the shark fin icon.
4. Under Capture menu, click Capture Filters
4. Open a Terminal window.
5. Use **`nslookup`** to issue some dns queries. Use a less common domain name.
6. Stop the capture by clicking the shark fin icon.
7. In filter box, type DNS to filter traffic to DNS query
8. Take some time to explore DNS traffic in the different panels.
9. **Make a screen shot of the packet capture DNS.jpg .**

Upload the screenshot (dns.jpg) and this file with your answers to Brightspace.

