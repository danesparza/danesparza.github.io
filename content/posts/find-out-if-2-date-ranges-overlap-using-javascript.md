---
date: "2011-07-19"
url: /2011/07/find-out-if-2-date-ranges-overlap-using-javascript
title: "Find out if 2 date ranges overlap using Javascript"
---
I recently discovered an ingeniously simple way to see if 2 date ranges overlap using only Javascript:

	var e1start = e1.start.getTime();
	var e1end = e1.end.getTime();
	var e2start = e2.start.getTime();
	var e2end = e2.end.getTime();
 
	return (e1start > e2start && e1start < e2end || e2start > e1start && e2start < e1end);

Simple, eh?

One more quick note:  ff you need extra help with designing your app with Javascript, you can't go wrong with this fantastic book from Douglas Crockford:

<a href="http://www.amazon.com/gp/product/0596517742/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0596517742&linkCode=as2&tag=theblogofdane-20&linkId=5HO5RDBRDBPNVCIZ"><img border="0" src="/img/javascript_goodparts.jpg" ><br/>JavaScript: The Good Parts</a><img src="http://ir-na.amazon-adsystem.com/e/ir?t=theblogofdane-20&l=as2&o=1&a=0596517742" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />
