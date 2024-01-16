---
title: "Using generic lists in code"
date: "2013-07-03"
url: /2013/07/using-generic-lists-in-code
categories:
  - "C#"
---
For best performance it's usually <a href="http://www.codinghorror.com/blog/2005/07/for-best-results-dont-initialize-variables.html">not a good idea to initialize variables</a> in .NET.  When it comes to software maintenance costs, List initialization is a different story, however.

I've seen 2 different patterns used when creating lists in C# code recently.  One is simply to declare the list and implicitly or explicitly initialize it to null:

	public List<SomeObject> MyListProperty { get; set; } // Implicitly null (Ok)
	List<SomeObject> MyOtherListProperty = null; // Explicitly null (Ok)

The other is to declare the list and initialize it to an empty list.  
	
	List<SomeObject> SomeList = new List<SomeObject>(); // Initialize to empty list (Better)

It's almost always better to initialize to an empty list unless your application is extremely memory sensitive (see Jon Skeet's <a href='http://stackoverflow.com/a/151950/19020'>answer here</a>, and understand that List uses an Array to manage data internally).  

When you initialize to an empty list, you eliminate null checks prior to using the list, and can immediately iterate using a `foreach` loop, check the `.Count` on the list or use methods like `.Any()` to see if there are items in the list.

