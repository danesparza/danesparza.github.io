---
date: "2011-06-15"
url: /2011/06/using-the-collectionassert-class-in-a-c-unit-test
title: "Using the CollectionAssert class in a C# unit test"
---
Unit testing with collections can be tricky, especially when you're trying to compare collections.  Enter the lowly 'CollectionAssert' class in C#:

	//  Assert:
	//  1.) These items are not null
	//  2.) These items are of type MyClass
	//  3.) The expected collection is the same as the actual collection
	CollectionAssert.AllItemsAreNotNull(actual);
	CollectionAssert.AllItemsAreInstancesOfType(actual, typeof(MyClass));
	CollectionAssert.AreEqual(expected, actual);
            
	//  Also:
	//  4.) The collection we expect to be filled shouldn't be the same as the empty one
	//  5.) The expected empty collection should be the same as the actual empty one
	CollectionAssert.AreNotEqual(expected, actualEmpty);
	CollectionAssert.AreEqual(expectedEmpty, actualEmpty);
