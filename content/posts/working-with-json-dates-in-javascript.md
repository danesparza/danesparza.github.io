---
date: "2010-09-29"
url: /2010/09/working-with-json-dates-in-javascript
title: "Working with JSON dates in Javascript"
---
<p>Working with JSON when passing data back and forth to a server is usually a joy - except when it comes to dates.  Try passing a 'datetime' field from C# through JSON and you'll end up with a field that looks like:  </p>

	/Date(1224043200000)/

<p>You'll also quickly discover that this is not a format that can simply be passed in to the javascript Date class via the constructor, and it can't be easily parsed by anything else.  At this point, many developers just punt and pass the properly formatted string back, instead of passing the date itself back.</p>

<p>I recently discovered there <em>is a nifty way to parse this date data</em> in Javascript:</p>

	var date = new Date(parseInt(jsonDate.substr(6)));

<p>The substr function takes out the "/Date(" part, and the parseInt function gets the integer and ignores the ")/" at the end. The resulting number is passed into the Date constructor. Note that we are NOT <a href="http://blogs.msdn.com/b/ericlippert/archive/2003/11/01/53329.aspx">using 'eval'</a> to parse the date.</p>

<p>Using this method, you can use all of the <a href="http://w3schools.com/jsref/jsref_obj_date.asp">date formatting methods</a> built in to Javascript: </p>

	date.toDateString();

