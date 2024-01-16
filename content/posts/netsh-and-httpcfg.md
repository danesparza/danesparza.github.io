---
date: "2014-03-03"
title: "netsh, Windows 2003 and httpcfg"
url: /2014/03/netsh-and-httpcfg
categories: 
---

In a [previous article](/2014/01/debugging-dotnet-network-and-certificate-issues/), I talked about working with HttpListener.  Remember: [HttpListener uses http.sys](http://msdn.microsoft.com/en-us/library/ms229710%28v%3Dvs.110%29.aspx). It's necessary to tell http.sys that a process & user should be allowed to listen to a certain port before that process starts listening.  This is called 'url reservation'.

In Windows Vista and Windows server 2008, the way to do url reservation is with [`netsh.exe`](http://support.microsoft.com/kb/242468).  My development workstation is Windows 7, so I used `netsh.exe` to get things working locally.  And of course -- I thought that I could use netsh.exe anywhere I needed to use HttpListener.  I was so wrong.  

#### Older machines use httpcfg.exe for port reservation
Much to my surprise netsh doesn't work the same way on all machines.  At my workplace we have Windows 2003 servers for QA, staging and production.  I discovered (the hard way) that I can't use `netsh.exe` to do my url/port reservation -- I have to use something called [httpcfg.exe](http://stackoverflow.com/questions/18221782/netsh-and-httpcfg).  The syntax is deceptively similar:

	httpcfg.exe set urlacl /u http://+:9001/ /a D:(A;;GX;;;NS)

Just like `netsh.exe`, I can specify the url/port to listen to, but the user assigned to that reservation is specified using [SDDL](http://msdn.microsoft.com/en-us/library/windows/desktop/aa379567.aspx) - which is very cryptic.  See that `D:(A;;GX;;;NS)` in the example, above?  That's actually saying 'give permission to the NETWORK SERVICE user'. Yeah.  Cryptic. 

To make life a little easier and help find the SDDL representation of the NETWORK SERVICE user, I used a handy tool called [HttpCfg ACL helper](http://dmitrygusev.blogspot.com/2009/07/host-wcf-service-in-windows-service.html).  That tool generates the httpcfg syntax that I needed -- including the SDDL.