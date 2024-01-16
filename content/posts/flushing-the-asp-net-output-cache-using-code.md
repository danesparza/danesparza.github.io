---
title: "Flushing the ASP.NET Output cache using code"
date: "2013-01-24"
url: /2013/01/flushing-the-asp-net-output-cache-using-code
categories:
  - "C#"
  - "ASP.NET"
---
When looking to speed up content that will be served from an ASP.NET MVC controller, one of the options I evaluate is the built in <a href="http://www.asp.net/mvc/tutorials/older-versions/controllers-and-routing/improving-performance-with-output-caching-cs">Output caching mechanism</a>.  Output caching is great because it's easy to setup, easy to maintain and it can give a big improvement for a small amount of code.

Sometimes, the cached item will need to be flushed (and regenerated / recached).   The <a href="http://msdn.microsoft.com/en-us/library/system.web.mvc.outputcacheattribute(v=vs.108).aspx">built in mechanisms for handling this</a> -- using cache expiration, or varying the cached output by one of many parameters -- handle most use cases pretty easily.

But other times I need to <strong>expire the Output cache programatically</strong>, and it's not entirely obvious how to do this, so I have documented it, here:


	//  Get the url for the action method:
	var staleItem = Url.Action("Action", "YourController", new
	{
	    Id = model.Id,
	    area = "areaname";
	});

	//  Remove the item from cache
	Response.RemoveOutputCacheItem(staleItem);


Where <em>"Action"</em> and <em>"YourController"</em> are the names of the Action method and controller that have the `[OutputCache]` attribute.  Notice that this example also passes route data (the <em>Id</em> and the area <em>"areaname"</em>) -- this is how you'll need to specify any parameters to the action method to build a complete url in <em>Url.Action</em>.

Note: that you can use this code from any part of your application -- you can expire the output cache of one controller/method from a completely different controlller/method.

Also, you'll need to remember to add the `Location=OutputCacheLocation.Server` parameter to the OutputCache attribute, like this:


	[OutputCache(Location=System.Web.UI.OutputCacheLocation.Server, Duration = 300, VaryByParam = "Id")]

