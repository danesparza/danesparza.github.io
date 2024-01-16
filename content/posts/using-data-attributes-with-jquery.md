---
date: "2014-01-22"
title: "Gotchya using data attributes with jQuery"
url: /2014/01/gotchya-using-data-attributes-with-jquery
categories: 
---

Today I discovered an interesting 'gotchya' when using [HTML5 data attributes](http://www.w3.org/html/wg/drafts/html/master/dom.html#embedding-custom-non-visible-data-with-the-data-*-attributes) with jQuery.  jQuery supports getting these data attributes with the [jQuery.data()](http://api.jquery.com/jquery.data/) syntax ... but beware:  **it will automatically strip hyphens and camel-case hyphenated attributes**.  

In other words, if you try to use the following HTML:

	<div data-role="page" data-last-value="43" data-hidden="true" data-options='{"name":"John"}'></div>

... the `data-last-value` attribute automatically becomes `lastValue` when accessing it in jQuery.  

This also introduces another behavior you should be aware of.  *You should never camel-case your own data attributes*, because jQuery will never be able to access them.  

Today I had this HTML: 

	<tr data-configId="22">
		...
	</tr>

and I was trying to access that data attribute with the following jQuery selector:

	$("tr").data("configId")

It wouldn't find the data attribute.  **When I changed the data-attribute to be all lower case, everything magically worked**.  