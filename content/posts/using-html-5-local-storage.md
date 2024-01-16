---
date: "2010-09-20"
url: /2010/09/using-html-5-local-storage
title: "Using HTML 5 local storage"
---
<a href="http://dev.w3.org/html5/webstorage/">HTML 5 includes a significant bump</a> in what the browser itself can store (up from 4k for total cookie storage to something on the order of 5-10mb).  Here's a simple code sample that illustrates how to access local storage in HTML5:

	//  First, make sure our browser supports HTML 5 local storage
	if (typeof(localStorage) == 'undefined' ) 
	{
		alert('Your browser does not support HTML5 localStorage. Try upgrading.');
	}
	else 
	{
		try 
		{
			//  saves to the database using key/value
			localStorage.setItem('name', 'Hello World!'); 
		} 
		catch (e) 
		{
			if (e == QUOTA_EXCEEDED_ERR)
			{
				//  data wasn’t successfully saved due to quota exceed so throw an error
				alert('Quota exceeded!'); 
			}
		}

		//  Write out the item from local storage:
		document.write(localStorage.getItem('name')); 

		//deletes the matching item from the database
		localStorage.removeItem('name'); 
	}


