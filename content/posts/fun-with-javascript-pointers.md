---
date: "2011-07-15"
url: /2011/07/fun-with-javascript-pointers
title: "Fun with Javascript pointers"
---
I just spent the last hour trying to debug some Javascript code that wasn't working the way I expected.  It turns out I was dealing with the <a href="http://en.wikipedia.org/wiki/Object_copy">shallow copying</a> behavior of Javascript.  If you deal in Javascript objects regularly, you need to know this information!

Here is a quick example that illustrates what I saw while debugging today:

```javascript
// First object, with a simple string property
args1 = {};
args1.test1 = "blah";

// Second object, 'created' from the first
args2 = args1;

// As you might expect...
// args2.test1 is "blah"

// Set the property to something different
// on the second object
args2.test1 = "boo!";

// But wait -- the objects are linked!
// args1.test1 is now "boo!" too!
```

As you can see -- the two objects ended up being 'linked' to each other due to the nature of how Javascript 'shallow copies' work with objects.

Spotting this in your code is the hard part.  The way to work around this is simple (as long as you're using jQuery):

```javascript
// First object, with a simple string property
args1 = {};
args1.test1 = "blah";

// Second object, deep copied from the first
args3 = $.extend(true, {}, args1)

// As you might expect...
// args3.test1 is "blah"

// Set the property to something different
// on the second object
args3.test1 = "boo!";

// The objects are not linked
// args1.test1 is "blah" 
```

For more information, see <a href="http://goo.gl/lwwNS">this answer on StackOverflow</a> (by John Resig himself).
