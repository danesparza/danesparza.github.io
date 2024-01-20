---
date: "2014-02-07"
title: "Debugging .NET network and certificate issues"
url: /2014/01/debugging-dotnet-network-and-certificate-issues
categories: 
---

Dear Reader, I learned some important things the hard way this week, so I thought I'd share the fruits of my agony.

<img class="img-responsive" src="/img/passfailed.gif" alt="It's been a heck of a week" />

### You must reserve ports when working with HttpListener
I'm used to letting [IIS](http://www.iis.net/) handle the details of hosting a web service for me, so I wasn't familiar with the ins and outs of netsh.  If you [self host with OWIN](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api), this apparently gets taken care of for you -- so you don't need to worry about it.  If you self-host with ServiceStack (like I did) be aware that [ServiceStack uses HttpListener](http://stackoverflow.com/a/14241463/19020).  

The basic idea is the operating system needs to make sure a given user has permission to start listening for network requests before it'll actually pass the network traffic to the listening application.  In fact until you configure this correctly, your service will never get hit, but calls will return with a 503 error.

For future reference: If your Windows service needs to listen on port 9001 and it's installed with the 'Network service' account, you'll need to run the following netsh command on the box (as an administrator, of course) before the service will actually be able to handle network requests:

	netsh http add urlacl url=http://+:9001/ user="NT AUTHORITY\NETWORK SERVICE"

I also had no idea that netsh was so finicky.  For example, there is a significant difference between `urlacl=http://+:port` and `urlacl=http://*:port` - one reserves any untaken IPs, the other reserves all known IPs.

You can see what urls have already been reserved using the command `netsh http show urlacl`

### If you press 'install certificate' in the browser, you're doing it wrong
When viewing the certificate in the browser, if you think pressing the 'install certificate' button will make your cert problems go away, you're mistaken.

<img class="img-responsive" src="http://www.poweradmin.com/help/sslhints/ie_viewca.png" alt="Internet explorer certificate details dialog" />

Instead, you need to open Microsoft Management console, add the 'Certificates' plugin, make sure you choose 'Computer account' (in the "this snap-in will always manage certificates for" dialog) and THEN install the certificate to the 'Trusted Root Certificate' store. More information on [Jeff Sander's blog](https://blogs.msdn.com/b/jpsanders/archive/2009/09/16/troubleshooting-asp-net-the-remote-certificate-is-invalid-according-to-the-validation-procedure.aspx?Redirected=true).

### You can trace network calls made with .NET automatically
While reading through the [certificate troubleshooting steps on Jeff Sander's blog](https://blogs.msdn.com/b/jpsanders/archive/2009/09/16/troubleshooting-asp-net-the-remote-certificate-is-invalid-according-to-the-validation-procedure.aspx?Redirected=true), I realized Jeff also walks you through turning on tracing for internal .NET network and security calls.  It's as easy as including some additional stuff in your application's config file:

```xml
<configuration>
    <system.diagnostics>
    <trace autoflush="true" />
    <sources>
        <source name="System.Net">
            <listeners>
                <add name="System.Net"/>
            </listeners>
    </source>
    <source name="System.Net.HttpListener">
            <listeners>
                <add name="System.Net"/>
            </listeners>
    </source>
    <source name="System.Net.Sockets">
            <listeners>
                <add name="System.Net"/>
            </listeners>
    </source>
    <source name="System.Net.Cache">
            <listeners>
                <add name="System.Net"/>
            </listeners>
    </source>
    </sources>
        <sharedListeners>
            <add name="System.Net" 
             type="System.Diagnostics.TextWriterTraceListener" 
             initializeData="System.Net.trace.log" 
             traceOutputOptions = "ProcessId, DateTime"/>
        </sharedListeners>
    <switches>
        <add name="System.Net" value="Verbose" />
        <add name="System.Net.Sockets" value="Verbose" />
        <add name="System.Net.Cache" value="Verbose" />
        <add name="System.Net.HttpListener" value="Verbose" />
    </switches>
    </system.diagnostics>
</configuration>
```

Using this method, I was able to easily see a certificate validation error with a self-signed cert that I was using.  Without it, I was flying blind.