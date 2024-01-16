---
date: "2011-06-03"
url: /2011/06/using-jquery-on-googles-cdn-but-having-a-plan-b
title: "Using jQuery on Google's CDN but having a plan B"
---
For personal projects or (even not so personal projects) <a href="http://encosia.com/3-reasons-why-you-should-let-google-host-jquery-for-you/">using Google's CDN hosted jQuery libraries is a no brainer</a>.  But what happens when you need a plan B?  Or what happens when you realize that you need to have a fallback plan <a href="http://stackoverflow.com/questions/1014203/best-way-to-use-googles-hosted-jquery-but-fall-back-to-my-hosted-library-on-goo/1014251#1014251">just in case Google is banned</a> in a country where you have website visitors?

Well, the answer is a <strong>pretty neat trick</strong> I just learned.  Taking advantage of the fact that Javascript is loaded synchronously -- you can have a set of script tags that look like this:

	<script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1.6.1/jquery.min.js"></script>
	<script type="text/javascript"> window.jQuery || document.write("<script src='/scripts/jquery-1.6.1.min.js'>\x3C/script>") </script>

Don't forget <a href="http://developer.yahoo.com/performance/rules.html">YSlow guidance</a> that says you should put your script tags near the bottom of your page!
