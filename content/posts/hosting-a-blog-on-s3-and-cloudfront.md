---
date: "2013-07-24"
title: "Hosting a blog on S3 and Cloudfront"
url: /2013/07/hosting-a-blog-on-s3-and-cloudfront
categories: 
---

Creating a blog that is easy-to-use, lightning fast, inexpensive, has fewer security problems than Wordpress, and never has downtime might seem impossible … but I think I've found a way.  

### Easy to use
[Hugo](https://github.com/spf13/hugo) is our foundation.  OK -- I'll admit that Hugo isn't exactly easy to use for a non-geek.  If you already know a bit of HTML & Javascript, it's a perfect fit for this operation.  Hugo generates a static site based on [Markdown](http://daringfireball.net/projects/markdown/syntax) based posts.  Comments for each post are taken care of using [Disqus](http://disqus.com/ ).  

Because Hugo is is a GoLang based binary, there is a single binary for each platform it runs on (including Windows).  No complicated installs -- just download and run.   

[Get started with Hugo](http://hugo.spf13.com/).

### Never has downtime
Now that you have a set of static html pages that represent your blog -- where to put them?  For that, we'll use [Amazon's S3](http://aws.amazon.com/s3/) storage service.  First, sign up and [create an S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) on AWS.  When setting it up, make sure it's [marked to 'enable website hosting'](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html).  

You can automate the deployment of your Hugo blog to S3. I use the free [S3 tool](http://s3.codeplex.com/) to automate this in a batch file.

After the bucket is setup and you have transferred your Hugo 'public' directory content to it, you should be able to navigate to it using an AWS assigned url like [http://danesparza.s3-website-us-east-1.amazonaws.com](http://danesparza.s3-website-us-east-1.amazonaws.com) (in the case of this blog).  Make sure that you set your document root to index.html.

### Lightning fast
In order to make the static content even faster, we can automatically distribute it on a CDN.  Since the entire blog is just static content at this point, it makes sense that it can be cached at several locations around the globe -- which is what AWS Cloudfront gives you automagically.  

Here's the trick to getting this working correctly with the site you've already got working in S3: When [creating your Cloudfront distribution](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/CreatingDownloadDistributions.html), set your Cloudfront origin to the S3 hostname.  In this case, that's danesparza.s3-website-us-east-1.amazonaws.com.  Set the 'alternate names' to whatever your blog hostname is.  In the case of this blog, I set it to:

	danesparza.net
	www.danesparza.net

AWS will assign a domain name.  In the case of this blog, it's: `d36829k8vm65ei.cloudfront.net`.  Make sure that you can access your site via this domain name before proceeding.  It might take several minutes for the distribution to be deployed and for this url to be ready.  

Make sure to set a default cache behavior with a Minimum TTL (time to live) of around 43200 -- that will make sure that your content is cached for a minimum of 12 hours.  In other words, if you make a change to your site, you wont see it go live for 12 hours.  If that's too long a time, you can adjust this TTL down.  The TTL is specified in seconds.

To setup the root domain (in the case of this blog: danesparza.net ) you'll need to use [AWS Route 53](http://aws.amazon.com/route53/) DNS service.  See [this blog article on root domain hosting](http://aws.typepad.com/aws/2012/12/root-domain-website-hosting-for-amazon-s3.html) for more information.

### Fewer security problems than Wordpress
Think about what we've setup now.  Unless an attacker steals your S3 credentials, the site that has been setup doesn't have any server side attack vectors because it's all static HTML.  There is nothing executing on the server side.  There is nothing to exploit.  This is [much more secure](http://www.cloudways.com/blog/10-wordpress-security-issues-how-to-fix-them/) than [Wordpress](http://techcrunch.com/2008/06/11/my-blog-was-hacked-is-yours-next-huge-wordpress-security-issues) 

### Inexpensive
We're using 2 AWS services (and possibly 3 if you opted for Route 53).  Each of them require that you pay for usage.  So how much money are we talking about? 

[S3 pricing](http://aws.amazon.com/s3/pricing/): Currently $.005 per 1000 requests

[Cloudfront pricing](http://aws.amazon.com/cloudfront/pricing/):  Depends on the region, but US is currently $.0075 per 10,000 requests

[Route 53 pricing](http://aws.amazon.com/route53/pricing/): $.50 per month per hosted zone / domain + $.50 for the first million queries)

Depending on the size of your site, you're talking less than $1/month for all of this.  

I call that inexpensive.
