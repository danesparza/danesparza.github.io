---
date: "2014-09-16"
url: /2014/09/using-windows-authentication-with-iisexpress
title: "Using Windows Authentication with IISExpress"
---
I do a lot of development with websites in Visual Studio 2013 nowadays. I've discovered that in order to use [IISExpress](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) with [Windows Authentication](http://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication), I had to jump through some hoops.  You may find yourself banging your head on the wall trying to get IISExpress to work with Windows auth -- so here are few tips for you.

### Update your web.config
Make sure your web.config file both enables windows authentication and also denies anonymous authentication.  `HttpContext.Current.User.Identity.Name` will be blank if the app falls through to anonymous authentication.  Your config should look something like this: 

	<authentication mode="Windows" />
	<authorization>
		<deny users="?"/>
	</authorization>

### Error 401.2 Unauthorized
Sometimes, you might get the `401.2 Unauthorized: Logon failed due to server configuration` error.  If you do, [verify that you have permission to view this directory or page based](http://technet.microsoft.com/en-us/library/bb727008.aspx) on the credentials you supplied.  Also make sure you have the authentication methods enabled on the Web server.

### Updating applicationhost.config
You also might find you have to update the IISExpress [applicationhost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) file (dont' worry -- I didn't know it either).  This is essentially the file version of the IIS configuration tool, where you can configure the web server itself.  Finding the applicationhost.config file can be tricky.  It might be in:

`%userprofile%\documents\iisexpress\config\applicationhost.config`

or 

`%userprofile%\my documents\iisexpress\config\applicationhost.config`

Once you find it, update the following lines (paying special attention to `enabled=true`):

	<windowsAuthentication enabled="true">
		<providers>
			<add value="Negotiate" />
			<add value="NTLM" />
	   	</providers>
	</windowsAuthentication>



