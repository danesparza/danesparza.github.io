---
date: "2010-09-06"
url: /2010/09/performing-a-join-using-linq
title: "Performing a join using LINQ"
---
So what do you do when you're using the nifty new 'entity framework' and you're getting data back with LINQ queries and you've reached the point where you want to get data back based on criteria in a related table? 

Well, if you're not using a stored proc to do it for you -- you've got to do a join. 

The join syntax in LINQ is similar to SQL syntax, but not quite the same.

Given the following relationships in the database...
<img src="http://media.tumblr.com/tumblr_l8c484u7391qzqijq.png" />

... the join would look something like this:

	var shoppingLists = from user in Users
									join listUsers in ShoppingListUsers
										on user.UserId equals listUsers.UserId
									join lists in ShoppingLists
										on listUsers.ShoppingListId equals lists.ShoppingListId
									where user.UserId == userToFind.UserId
									select lists;

	return shoppingLists;
