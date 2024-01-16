---
date: "2010-09-10"
url: /2010/09/how-do-delete-data-from-multiple-tables-at-once-in-sql
title: "How to delete data from multiple tables at once in SQL"
---
The short answer is you can't.  But there is a very nice workaround.

I was perusing StackOverflow and came across this article talking about how to capture information from deleted rows and then use them in a join using the <b>deleted</b> pseudo table:

<a href="http://stackoverflow.com/questions/783726/how-do-i-delete-from-multiple-tables-using-inner-join-in-sql-server">How do I delete from multiple tables using INNER JOIN in SQL server - Stack Overflow</a>

	begin transaction;

	   declare @deletedIds table ( id int );

	   delete t1
	   /* Notice this next line is using the 'deleted' pseudo table: */
	   output deleted.id into @deletedIds;
	   from table1 t1
		join table2 t2
		  on t2.id = t1.id
		join table3 t3
		  on t3.id = t2.id;

	   delete t2
	   from table2 t2
		join @deletedIds d
		  on d.id = t2.id;

	   delete t3
	   from table3 t3 ...

	commit transaction;

