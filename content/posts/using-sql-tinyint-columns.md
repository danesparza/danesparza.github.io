---
date: "2010-09-16"
url: /2010/09/using-sql-tinyint-columns
title: "Using SQL tinyint columns"
---
<p>Note to self:  When using (sql) tinyint columns and doing something like this:</p>

	DataSet ds = GetDataSetFromStoredProc("stored_proc_name", new SqlParameter("@parameter_name", value_id));

	var retItems = from dstable in ds.Tables[0].AsEnumerable()
					select new ClassThing()
					{
						PrimaryKeyId = dstable.Field<int>("some_column_name")
					};

<p>the correct type to cast to is 'byte'.  Byte is apparently an 8-bit unsigned integer, which is just perfect.</p>

<p>For future reference, it turns out there is a very handy <a href="http://msdn.microsoft.com/en-us/library/ms131092.aspx">SQL to CLR data type map</a> available on MSDN.</p>
