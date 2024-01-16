---
date: "2010-07-13"
url: /2010/07/asp-net-mvc2-custom-validation-helper
title: "ASP.NET MVC2 custom validation helper"
---
RedGate reflector and ASP.NET MVC2 rock.

Using this extension method:</p>

	/// <summary>
	/// If the given model field has validation errors, this will emit the given CSS class name
	/// </summary>
	/// <typeparam name="TModel"></typeparam>
	/// <typeparam name="TProperty"></typeparam>
	/// <param name="htmlHelper"></param>
	/// <param name="expression"></param>
	/// <param name="cssClassToEmit"></param>
	/// <returns></returns>
	public static MvcHtmlString ValidationCSSClassFor<TModel, TProperty>(this HtmlHelper<TModel> htmlHelper, Expression<Func<TModel, TProperty>> expression, string cssClassToEmit)
	{
		MvcHtmlString htmlString = null;

		//  Figure out the expression text from the LambdaExpression using our nifty helper
		//  (thank God for RedGate reflector or I would have never figured this one out)
		string expressionText = ExpressionHelper.GetExpressionText((LambdaExpression) expression);

		//  Get the full field name from the expression string:
		string fullHtmlFieldName = htmlHelper.ViewContext.ViewData.TemplateInfo.GetFullHtmlFieldName(expressionText);

		//  Get the model state from the field name:
		ModelState modelState = htmlHelper.ViewData.ModelState[fullHtmlFieldName];

		//  Check to see if we have errors.  If we don't, just get out:
		ModelErrorCollection errors = (modelState == null) ? null : modelState.Errors;
		ModelError error = ((errors == null) || (errors.Count == 0)) ? null : errors[0];
		if((error == null))
		{
			return null;
		}

		//  At this point, we have errors -- but if we don't have a CSS class we're supposed to emit
		//  ... well, we're just SOL
		if(!string.IsNullOrEmpty(cssClassToEmit))
		{
			htmlString = MvcHtmlString.Create(cssClassToEmit);
		}

		return htmlString;
	}

I'm now able to do the following awesome sauce in an ASP.NET MVC2 view:

	<div id="email_tip" class="tip <%: Html.ValidationCSSClassFor(m=>m.Email, "error") %>" style="display: none;float: left;">
		<%: Html.ValidationMessageFor(m=>m.Email) %>
	</div>

