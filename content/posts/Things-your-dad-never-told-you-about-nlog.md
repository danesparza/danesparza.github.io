---
date: "2014-06-04"
url: /2014/06/things-your-dad-never-told-you-about-nlog
title: "Things your Dad never told you about NLog"
---
I've had cause to work with NLog a lot lately.  In working with it, I have discovered some surprises and tricks that I thought I would share.

### Log levels matter
You should think carefully about your log levels as you start adding logging to your app.  `Trace` should probably be used sparingly, because when you turn on log level 'Trace' that means you're pretty much logging EVERYTHING.  Log level `Debug` or `Info` might be a better level to start with.  

This makes a bit more sense as you think about the types of things you might be logging in production.  If you log very large detailed messages -- like complete dumps of email messages with attachments -- you might want to log just those few large things using the `Trace` log methods.  That way, when you change the log level to `Debug` in the nlog.config, you won't log the gigantic messages to your log.

### Automatic log rotation
Nlog can automatically rotate your logs.  It can be configured to keep the last 'n' number of days and delete the rest automatically.  This can be very useful in production, where you don't want System Operations to worry about rotating logs or deleting old logs. 

Here is a sample nlog.config that handles log rotation automatically:

	<?xml version="1.0" encoding="utf-8" ?>
	<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
	      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

	    <!-- 
	  See http://nlog-project.org/wiki/Configuration_file 
	  for information on customizing logging rules and outputs.
	   -->
	    <targets>
	        <!-- add your targets here -->
	        <target name="f" xsi:type="File"
	           layout="${longdate} ${logger} ${message} ${exception:format=tostring}"
	           fileName="${basedir}/logs/current.log"
	           archiveFileName="${basedir}/logs/archive.{#}.log"
	           archiveEvery="Day"
	           archiveNumbering="Rolling"
	           maxArchiveFiles="7"
	           concurrentWrites="true"
	           keepFileOpen="false"
	           encoding="iso-8859-2" />

	    </targets>

	    <!-- Add your logging rules here.  -->
	    <rules>
	        <logger name="*" minlevel="Info" writeTo="f" />
	    </rules>
	    
	</nlog>

### Programmatically find where the log is
You can use the NLog API to find out where the current log is programatically.  This can be useful if you want to email the current log for some reason.  

To get the current log file, you need to know the current log file name in the nlog.config.  

	//	Find the correct target
	var fileTarget = (FileTarget)LogManager.Configuration.FindTargetByName("f");

	//	Using the target, get the full path to the log file
	string retval = Path.GetFullPath(fileTarget.FileName.Render(logEventInfo));

### Logging an exception requires an update to your layout
You may have seen the `ErrorException`, `WarnException` (and other) methods on the Nlog object.  They are used to log exception stack traces in your code automatically, like this:

	try
	{
	    //	... Some code that might cause an exception
	}
	catch (WebException webEx)
	{
	    logger.ErrorException("There was a serious problem communicating with on the network", webEx);
	}

In this case, the `webEx` exception object is the one we'd like to log, along with the message.  The problem is, if we don't include an `${exception}` placeholder to our layout, the exception stack trace won't get logged.  

For example, if your current layout was:
	
	<target name="Errors" xsi:type="File" fileName="${logDirectory}/ErrorLog.txt" layout="${longdate} ${message}"/>

You would need to change it to:

	<target name="Errors" xsi:type="File" fileName="${logDirectory}/ErrorLog.txt" layout="${longdate} ${message} ${exception:format=tostring}"/>

In order to log exceptions properly.

### You can log all network traffic
This is kind of bananas.  I discovered this neat trick when troubleshooting a web service I had written.  In a nutshell you can log all low level network traffic -- even when you are only indirectly calling network libraries.  This can help you diagnose SSL certificate errors, among other things.

Adding the following to your web.config/app.config file will log all windows sockets traffic for you automatically:

	<system.diagnostics>
	    
	    <sources>
	        <source name="System.Net" switchValue="All">
	            <listeners>
	                <add name="nlog" />
	            </listeners>
	        </source>
	        <source name="System.Net.Sockets" switchValue="All">
	            <listeners>
	                <add name="nlog" />
	            </listeners>
	        </source>
	    </sources>
	    
	    <sharedListeners>
	        <add name="nlog" type="NLog.NLogTraceListener, NLog" />
	    </sharedListeners>
	</system.diagnostics>

### You can easily log entire objects
Using [ServiceStack.Text](https://github.com/ServiceStack/ServiceStack.Text)'s `Dump()` extension method, you can easily serialize and Dump entire objects to your log file.  If you're not already using ServiceStack.Text, you can install it easily via NuGet using the command:

	PM> Install-Package ServiceStack.Text

Then, to log an entire object in code is as simple as

	logger.Debug("Initializing settings with object: {0}", someobject.Dump());

Be careful if you have objects that reference each other -- you can inadvertently cause a stack overflow if you're not careful.  To work around this issue, create an anonymous object with only the properties you want to log and then call `Dump()` on that object.