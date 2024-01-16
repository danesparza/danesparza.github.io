---
date: "2011-05-17"
url: /2011/05/using-the-dig-dns-tool-on-windows-7
title: "Using the dig dns tool on Windows 7"
---

Traditionally, nslookup is the tool of choice when trying to find out information about IP addresses or DNS information in Windows. In the Linux world, nslookup <a href="http://www.linuxquestions.org/questions/linux-networking-3/why-nslookup-is-deprecated-122337/">has been deprecated</a> for a long time. The preferred way to query for dns information from the command line is the <a href="http://en.wikipedia.org/wiki/Domain_Information_Groper"><strong>Domain Information Groper</strong></a> or '<strong>dig</strong>' dns tool.

Interested in learning more about DNS and dig? Check out this book:

[![Alt text](/img/dns_bind.jpg)](http://www.amazon.com/gp/product/0596100574/ref=as_li_ss_il?ie=UTF8&camp=1789&creative=390957&creativeASIN=0596100574&linkCode=as2&tag=theblogofdane-20)

### What can you do with dig?

Using dig, you can find out what a particular dns server thinks the given host's IP address should be, including a lot of other information that is also very helpful.  For example, running this command:

<!--more-->

```
dig www.cagedtornado.com
```

Results in a whole host of information coming back:

    ; <<>> DiG 9.8.0-P1 <<>> www.cagedtornado.com
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15102
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 10, AUTHORITY: 0, ADDITIONAL: 0

    ;; QUESTION SECTION:
    ;www.cagedtornado.com.          IN      A

    ;; ANSWER SECTION:
    www.cagedtornado.com.   3600    IN      CNAME   d1go1kcsby4lr.cloudfront.net.
    d1go1kcsby4lr.cloudfront.net. 60 IN     CNAME   d1go1kcsby4lr.sfo4.cloudfront.net.
    d1go1kcsby4lr.sfo4.cloudfront.net. 60 IN A      216.137.37.65
    d1go1kcsby4lr.sfo4.cloudfront.net. 60 IN A      216.137.37.55
    d1go1kcsby4lr.sfo4.cloudfront.net. 60 IN A      216.137.37.68
    d1go1kcsby4lr.sfo4.cloudfront.net. 60 IN A      216.137.37.32
    d1go1kcsby4lr.sfo4.cloudfront.net. 60 IN A      216.137.37.53
    d1go1kcsby4lr.sfo4.cloudfront.net. 60 IN A      216.137.37.11
    d1go1kcsby4lr.sfo4.cloudfront.net. 60 IN A      216.137.37.209
    d1go1kcsby4lr.sfo4.cloudfront.net. 60 IN A      216.137.37.31

    ;; Query time: 210 msec
    ;; SERVER: 192.168.0.1#53(192.168.0.1)
    ;; WHEN: Tue May 17 07:58:01 2011
    ;; MSG SIZE  rcvd: 241

Note that the information returned includes a summary section at the top, and includes feedback on whether or not the query had an answer, and how many answers were returned. I'll explain what each of these sections means in a bit.

### Installing dig on windows 7

Installing dig on Windows 7 is as simple as going to <a href="http://www.isc.org/software/bind">ISC's BIND site</a> and downloading the Windows distribution. Unzip into your directory of choice.

When running `dig.exe` for the first time, you may get the following error message:

<em>The application has failed to start because its side-by-side configuration is incorrect. Please see the application event log or use the command-line sxstrace.exe tool for more detail.</em>

If you see this message, it's most likely that dig is trying to use the VC80 CRT restributable, and it's not installed yet on your system. (You can check the application event log to make sure). If that's the case, just run the file vcredist_x86.exe, included in the distribution. For more information on what this error message is all about, you can check out <a href="http://blogs.msdn.com/b/junfeng/archive/2006/04/14/576314.aspx">this blog post from Junfeng Zhang</a>.

### Using dig on Windows 7

Using dig, you can see what a specific DNS server thinks an address should be:

    dig @ns1.google.com www.google.com

    ; <<>> DiG 9.8.0-P1 <<>> @ns1.google.com www.google.com
    ; (1 server found)
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 30428
    ;; flags: qr aa rd; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 0
    ;; WARNING: recursion requested but not available

    ;; QUESTION SECTION:
    ;www.google.com.                        IN      A

    ;; ANSWER SECTION:
    www.google.com.         604800  IN      CNAME   www.l.google.com.
    www.l.google.com.       300     IN      A       74.125.224.50
    www.l.google.com.       300     IN      A       74.125.224.49
    www.l.google.com.       300     IN      A       74.125.224.48
    www.l.google.com.       300     IN      A       74.125.224.51
    www.l.google.com.       300     IN      A       74.125.224.52

    ;; Query time: 74 msec
    ;; SERVER: 216.239.32.10#53(216.239.32.10)
    ;; WHEN: Tue May 17 16:56:54 2011
    ;; MSG SIZE  rcvd: 132

You can do a reverse lookup (which means to take an IP address and find its fully qualified host name):

    dig -x 97.74.104.201


    ; <<>> DiG 9.8.0-P1 <<>> -x 97.74.104.201
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 24181
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 3, ADDITIONAL: 2

    ;; QUESTION SECTION:
    ;201.104.74.97.in-addr.arpa.    IN      PTR

    ;; ANSWER SECTION:
    201.104.74.97.in-addr.arpa. 3600 IN     PTR     corpweb-v101.prod.mesa1.secureserver.net.

    ;; AUTHORITY SECTION:
    74.97.in-addr.arpa.     3600    IN      NS      cns3.secureserver.net.
    74.97.in-addr.arpa.     3600    IN      NS      cns2.secureserver.net.
    74.97.in-addr.arpa.     3600    IN      NS      cns1.secureserver.net.

    ;; ADDITIONAL SECTION:
    cns1.secureserver.net.  3477    IN      A       208.109.255.100
    cns2.secureserver.net.  3477    IN      A       216.69.185.100

    ;; Query time: 28 msec
    ;; SERVER: 172.31.250.11#53(172.31.250.11)
    ;; WHEN: Tue May 17 16:58:53 2011
    ;; MSG SIZE  rcvd: 187

You can find a domain's mail servers:

    dig yahoo.com MX

    ; <<>> DiG 9.8.0-P1 <<>> yahoo.com MX
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 23047
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 13, AUTHORITY: 7, ADDITIONAL: 7

    ;; QUESTION SECTION:
    ;yahoo.com.                     IN      MX

    ;; ANSWER SECTION:
    yahoo.com.              1800    IN      MX      1 l.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 m.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 n.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 a.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 b.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 d.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 e.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 f.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 g.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 h.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 i.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 j.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 k.mx.mail.yahoo.com.

    ;; AUTHORITY SECTION:
    yahoo.com.              5014    IN      NS      ns5.yahoo.com.
    yahoo.com.              5014    IN      NS      ns2.yahoo.com.
    yahoo.com.              5014    IN      NS      ns8.yahoo.com.
    yahoo.com.              5014    IN      NS      ns1.yahoo.com.
    yahoo.com.              5014    IN      NS      ns3.yahoo.com.
    yahoo.com.              5014    IN      NS      ns6.yahoo.com.
    yahoo.com.              5014    IN      NS      ns4.yahoo.com.

    ;; ADDITIONAL SECTION:
    a.mx.mail.yahoo.com.    1800    IN      A       67.195.168.31
    b.mx.mail.yahoo.com.    1800    IN      A       74.6.136.65
    d.mx.mail.yahoo.com.    1800    IN      A       209.191.88.254
    e.mx.mail.yahoo.com.    1800    IN      A       67.195.168.230
    f.mx.mail.yahoo.com.    1800    IN      A       98.137.54.237
    g.mx.mail.yahoo.com.    1800    IN      A       98.137.54.238
    h.mx.mail.yahoo.com.    1800    IN      A       66.94.236.34

    ;; Query time: 29 msec
    ;; SERVER: 172.31.250.11#53(172.31.250.11)
    ;; WHEN: Tue May 17 17:04:50 2011
    ;; MSG SIZE  rcvd: 507

### Understanding the output

Dig's output typically has 5 sections:

#### Header section

Here, dig tells us its version number, the query it just got sent, and a summary of the information it got back. Pay close attention to the numbers beside query, answer, and authority: These describe the number of queries dig processed, the number of answers it got back (this might be 0 for items that don't exist), and the number of authoritive answers it got back. <a href="http://en.wikipedia.org/wiki/Domain_Name_System#Authoritative_name_server">If a DNS server is the primary or secondary nameserver for a given domain, it can return authoritive answers</a>.

    ; <<>> DiG 9.8.0-P1 <<>> yahoo.com MX
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 23047
    ;; flags: qr rd ra; QUERY: 1, ANSWER: 13, AUTHORITY: 7, ADDITIONAL: 7

#### Question section

Pretty self explanatory. Here, dig describes in detail what it's looking for.

    ;; QUESTION SECTION:
    ;yahoo.com.                     IN      MX

#### Answer section

Here, dig tells you what its found, including <a href="http://en.wikipedia.org/wiki/Time_to_live">TTL</a> information for each of the items. It looks something like this:

    ;; ANSWER SECTION:
    yahoo.com.              1800    IN      MX      1 l.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 m.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 n.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 a.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 b.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 d.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 e.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 f.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 g.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 h.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 i.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 j.mx.mail.yahoo.com.
    yahoo.com.              1800    IN      MX      1 k.mx.mail.yahoo.com.

#### Authority section

The authority section tells us what DNS servers can provide an authoritative answer to our query. In this example, isc.org has three name servers. You can toggle this section of the output using the +[no]authority option.

    ;; AUTHORITY SECTION:
    yahoo.com.              5014    IN      NS      ns5.yahoo.com.
    yahoo.com.              5014    IN      NS      ns2.yahoo.com.
    yahoo.com.              5014    IN      NS      ns8.yahoo.com.
    yahoo.com.              5014    IN      NS      ns1.yahoo.com.
    yahoo.com.              5014    IN      NS      ns3.yahoo.com.
    yahoo.com.              5014    IN      NS      ns6.yahoo.com.
    yahoo.com.              5014    IN      NS      ns4.yahoo.com.

#### Additional section

If dig gets any additional information back, it will appear here. This section of the output can be toggled with the +[no]additional option.

    ;; ADDITIONAL SECTION:
    a.mx.mail.yahoo.com.    1800    IN      A       67.195.168.31
    b.mx.mail.yahoo.com.    1800    IN      A       74.6.136.65
    d.mx.mail.yahoo.com.    1800    IN      A       209.191.88.254
    e.mx.mail.yahoo.com.    1800    IN      A       67.195.168.230
    f.mx.mail.yahoo.com.    1800    IN      A       98.137.54.237
    g.mx.mail.yahoo.com.    1800    IN      A       98.137.54.238
    h.mx.mail.yahoo.com.    1800    IN      A       66.94.236.34
