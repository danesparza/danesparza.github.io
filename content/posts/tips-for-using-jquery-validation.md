---
date: "2010-10-05"
url: /2010/10/tips-for-using-jquery-validation
title: "Tips for using jQuery validation"
---
<p>
The <a href="http://bassistance.de/jquery-plugins/jquery-plugin-validation/">jQuery validation plugin</a> is very easy to use, <a href="http://docs.jquery.com/Plugins/Validation">the docs</a> are pretty dang good, and <a href="http://jquery.bassistance.de/validate/demo/">the examples</a> are top notch.  However, there can be a few 'gotchyas' when working with the plugin.  
</p>

<p>First, if you're <a href="http://docs.jquery.com/Plugins/Validation#Example">using the 'easy' validation method</a>, <b>use a form</b> to wrap all of the fields you'd like to validate.  The form doesn't have to have an action, but it should have a unique 'id' attribute. 
</p>

<p>
Also, <b>all of your form elements should have a 'name' attribute</b> in addition to the 'id' attribute you'd normally use to reference them in the DOM.  The 'name' attribute is normally only used when submitting form elements -- but it needs to be there for jQuery validation even if you don't plan on submitting the form.
</p>

	<div id="content" style="padding: 10px;">
		<form id="frmTest" method="get">
			Lots of other content here<br />
			Lots of other content here<br />
			Lots of other content here<br />
			Lots of other content here<br />
			Lots of other content here<br />
			Lots of other content here<br />
			<input id="txtEmail" name="txtEmail" type="text" class="required email"/>
			<br />
			<input id="txtName" name="txtName" type="text" class="required" minlength="2"/>
			<br />
			Other content
			<br />
			<button id="btnValidate" type="button">Test validation</button>
		</form>
	</div>

<p><b>You can also use a custom 'error' CSS class</b>.  This can be useful when your CSS has already defined a class called 'error' (like <a href="http://www.blueprintcss.org/">Blueprint</a> does) and you don't want to use that CSS class for form validation errors.  jQuery validation uses the 'error' class by default.  To override this, you can define a different CSS class to use:</p>

	label.invalid
	{
		color: Red;
		font-style: italic;
		padding: 1px;
		margin: 0px 0px 0px 5px;
	}

	$(document).ready(function ()
	{
		//  Bind validation rules
		$("#frmTest").validate({
			errorClass: "invalid"
		});

		//  Bind click handler:
		$("#btnValidate").click(function ()
		{
			if ($("#frmTest").valid())
				alert("Valid");
		});
	});

If you need extra help with designing your app with Javascript, you can't go wrong with this fantastic book from Douglas Crockford:

<a href="http://www.amazon.com/gp/product/0596517742/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0596517742&linkCode=as2&tag=theblogofdane-20&linkId=5HO5RDBRDBPNVCIZ"><img border="0" src="/img/javascript_goodparts.jpg" ><br/>JavaScript: The Good Parts</a><img src="http://ir-na.amazon-adsystem.com/e/ir?t=theblogofdane-20&l=as2&o=1&a=0596517742" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />


