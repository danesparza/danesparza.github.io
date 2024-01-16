---
date: "2013-11-19"
title: "Hosting a blog on S3 - cheaper and simpler"
url: /2013/11/cheaper-hosting-on-s3
categories: 
---

[A few months ago](http://www.danesparza.net/2013/07/hosting-a-blog-on-s3-and-cloudfront/), I covered how to setup a blog using S3, Cloudfront and Route 53 that was dirt cheap, reliable, and fast.  Today, I'm going to walk through an even simpler and cheaper setup that is just as fast

### Overview
[Hugo](https://github.com/spf13/hugo) is (still) our foundation. Hugo generates a static site based on [Markdown](http://daringfireball.net/projects/markdown/syntax) based posts.  [Getting started with Hugo](http://hugo.spf13.com/overview/quickstart) is a breeze -- it's not as complicated as [Jekyll](http://jekyllrb.com/) or [Octopress](http://octopress.org/).  Comments for each post are taken care of using [Disqus](http://disqus.com/).  

Because Hugo is is a [GoLang](http://golang.org/) based binary, there is a single binary for each platform it runs on (including Windows).  No complicated installs -- just download and run.

### Storage on Amazon S3
Now that you have a set of static html pages that represent your blog -- where to put them?  For that, we'll use [Amazon's S3](http://aws.amazon.com/s3/) storage service.  First, sign up and [create an S3 bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) on AWS.    

#### S3 Bucket setup
Setting up an Amazon S3 bucket is fairly straightforward, but there are some important things to remember:

1. Your bucket name should match your website name.  (For this site, my bucket name is `www.danesparza.net`)
2. When setting up your bucket, under 'Static website hosting' make sure it's [marked to 'enable website hosting'](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) and set your index document to `index.html`

After the bucket is setup and you have transferred your Hugo 'public' directory content to it, you should be able to navigate to it using an AWS assigned url like http://www.danesparza.net.s3-website-us-east-1.amazonaws.com (in the case of this blog). 

#### Protip: Automatic refresh and deploy
You can automate the deployment of your Hugo blog to S3. I use the free [S3 tool](http://s3.codeplex.com/) to automate this in a batch file.

#### Protip: Store locally in Dropbox
For extra awesome, store your entire blog (content/design/published site) in a Dropbox folder.  Dropbox [automatically keeps track of your past file versions](https://www.dropbox.com/help/122/en) -- so you can roll back to a previous version of a blog post in a pinch.

### Lightning fast
In order to make the static content even faster, we can automatically distribute it on a CDN.  Since the entire blog is just static content at this point, it makes sense that it can be cached at several locations around the globe.  To make DNS and CDN distribution super simple, setup an account on [Cloudflare](https://www.cloudflare.com/overview).  Cloudflare will provide you with free tools to manage your domain's DNS information, [protect you from hacker attacks](https://www.cloudflare.com/features-security) and automatically distribute your content on their [content distribution network](https://www.cloudflare.com/features-cdn).

To setup your site in Cloudflare, simply create a [CNAME](https://support.cloudflare.com/hc/en-us/articles/200169046-How-do-I-add-a-CNAME-record-) record for your site that points `www.yourdomain.com` to your AWS assigned S3 bucket url.  

For this site, that means creating a CNAME that points `www` to `www.danesparza.net.s3-website-us-east-1.amazonaws.com`

### Fewer security problems than Wordpress
Think about what we've setup now.  Unless an attacker steals your S3 credentials, the site that has been setup doesn't have any server side attack vectors because it's all static HTML.  There is nothing executing on the server side.  There is nothing to exploit.  This is [much more secure](http://www.cloudways.com/blog/10-wordpress-security-issues-how-to-fix-them/) than [Wordpress](http://techcrunch.com/2008/06/11/my-blog-was-hacked-is-yours-next-huge-wordpress-security-issues) 

### Inexpensive
We're using just 1 AWS service now.  Each AWS service requires that you pay for usage.  So how much money are we talking about? 

[S3 pricing](http://aws.amazon.com/s3/pricing/): Currently $.005 per 1000 requests + storage costs

Depending on the size of your site, you're most likely talking less than $.25/month for all of this.  

I call that inexpensive.
