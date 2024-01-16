---
date: "2010-09-12"
url: /2010/09/using-the-jquery-ui-transfer-effect
title: "Using the jQuery UI 'transfer' effect"
---
When using the jQuery UI 'transfer' effect for the first time, you might think the effect isn't working.  The documentation is sparse, but the call looks simple enough:

	//  Use transfer effect 
	$("#txtProduct").effect("transfer", { to: $("#test") }, 1000);

If you add this to your page, add the html elements to make this work, and then run this code you'll notice ... absolutely nothing.  

That's because you need to style the actual 'transfer effect' itself.  (The docs actually <a href="http://docs.jquery.com/UI/Effects/Transfer">say this</a>, although you might not be looking for it).  Styling is as simple as:

	.ui-effects-transfer { border: 2px dotted gray; }

