---
date: "2010-10-19"
url: /2010/10/using-gravatar-images-with-c-asp-net
title: "Using Gravatar images with C# / ASP.NET"
---
Including profile pictures in your application from the nifty (and free!) <a href="http://en.gravatar.com/">gravatar</a> service is very easy, assuming you've already got an 'email' field for your users that you're tracking.

To use the system, you just need to form a url that points to the user's profile image (using a hash of their email address) and use that url in an image tag.  The url looks something like this:

Format:
	http://www.gravatar.com/avatar/{md5 hash}

Example:
	http://www.gravatar.com/avatar/73543542128f5a067ffc34305eefe48a

### The basics
But how do you create the MD5 hash of the email address?  No problem.  You can use .NET's built-in crytography libraries to help you.  Here is a nice helper function that takes an email address and forms the hash you'll need in the gravatar url:

<!--more-->

	using System.Security.Cryptography;

	/// Hashes an email with MD5.  Suitable for use with Gravatar profile
	/// image urls
	public static string HashEmailForGravatar(string email)
	{
		// Create a new instance of the MD5CryptoServiceProvider object.  
		MD5 md5Hasher = MD5.Create();
 
		// Convert the input string to a byte array and compute the hash.  
		byte[] data = md5Hasher.ComputeHash(Encoding.Default.GetBytes(email));
 
		// Create a new Stringbuilder to collect the bytes  
		// and create a string.  
		StringBuilder sBuilder = new StringBuilder();
 
		// Loop through each byte of the hashed data  
		// and format each one as a hexadecimal string.  
		for(int i = 0; i < data.Length; i++)
		{
			sBuilder.Append(data[i].ToString("x2"));
		}
 
		return sBuilder.ToString();  // Return the hexadecimal string. 
	}


Using the returned hash in the url is very simple.  Just place the formatted url into an image tag:

	//  Compute the hash
	string hash = HashEmailForGravatar(email);

	//  Assemble the url and return
	return string.Format("http://www.gravatar.com/avatar/{0}", hash);

### Customizing the output
Pass the 'size' querystring parameter if you don't want to use the default image size of 80px by 80px.

	http://www.gravatar.com/avatar/73543542128f5a067ffc34305eefe48a?size=56

<img src="http://www.gravatar.com/avatar/73543542128f5a067ffc34305eefe48a?size=56" alt="" />

	http://www.gravatar.com/avatar/73543542128f5a067ffc34305eefe48a?size=120

<img src="http://www.gravatar.com/avatar/73543542128f5a067ffc34305eefe48a?size=120" alt="" /> 

One last tip:  If the hashed email address can't be found, the following (unattractive) image will be displayed as a placeholder:

<img src="http://www.gravatar.com/avatar/73543542128f5a067ffc34305eefe48b" alt="" />

But you can pass in your own image to be used as a default, instead:

	http://www.gravatar.com/avatar/73543542128f5a067ffc34305eefe48b?d=http://media.tumblr.com/tumblr_lak5phfeXz1qzqijq.png

<img src="http://www.gravatar.com/avatar/73543542128f5a067ffc34305eefe48b?d=http://media.tumblr.com/tumblr_lak5phfeXz1qzqijq.png" alt="" />
