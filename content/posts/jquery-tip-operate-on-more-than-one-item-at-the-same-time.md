---
date: "2011-01-24"
url: /2011/01/jquery-tip-operate-on-more-than-one-item-at-the-same-time
title: "jQuery tip:  Operate on more than one item at the same time"
---
Sometimes it's useful to perform an action on more than one item on the page at the same time - like when hiding or showing a series of elements all at the same time.  With jQuery selector syntax, this is very easy.  First, if you're not familiar with basic CSS or <a href="http://api.jquery.com/category/selectors/">jQuery selectors</a>, you should probably start there.  Once you're familiar with that basic syntax however, you'll be happy to know you can combine selectors with a comma (using the <a href="http://api.jquery.com/multiple-selector/">'multiple selector' syntax</a>), like this:

	$("#itemone, #itemtwo").hide();
	$("#itemthree, #itemfour").show();

