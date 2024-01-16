---
date: "2010-09-13"
url: /2010/09/creating-a-cdn-with-amazon-cloudfront
title: "Creating a CDN with Amazon Cloudfront"
---
Amazon's <a href="http://aws.amazon.com/cloudfront/">Cloudfront service</a> is truly awesome.  It's a powerful, flexible, inexpensive way to get a content distribution network up and running so that you can play with the big boys on a startup budget.

Getting up and running is a snap.  First, make sure you've <a href="http://aws.amazon.com/">signed up for Amazon web services</a> S3 and Cloudfront services.  Signing up is free, but understand that Amazon charges modest fees for both <a href="http://aws.amazon.com/s3/#pricing">S3</a> and <a href="http://aws.amazon.com/cloudfront/#pricing">Cloudfront</a>.  

(To give you an idea of what kind of costs a tiny website like this will incur:  It's costing me all of $.04 this month to host static files with Cloudfront).

First, in the AWS dashboard create a 'bucket' in S3.  A bucket is just a place to host files.

<img src="http://media.tumblr.com/tumblr_l8pw31g9zu1qzqijq.png" />

Next, create any folders you'd like to organize your files.  I have created 2 folders:  'Scripts' and 'Styles'.

<img src="http://media.tumblr.com/tumblr_l8pwdo9pHH1qzqijq.png" />

Next, double click on one of the folders to select it (kind of like windows explorer).  Then, click 'upload' to begin the file selection and upload process.

<img src="http://media.tumblr.com/tumblr_l8pwfgiOLE1qzqijq.png" />

<strong>Now, this part is kind of important:</strong> Be sure to set the permissions the files to 'Make everything public'.  You actually need to check the box.  

<img src="http://media.tumblr.com/tumblr_l8pwilCLZ21qzqijq.png" />

After your files have been uploaded, it's time for the really cool part. Switch over to the 'Cloudfront' dashboard in Amazon Web Services and click the 'create distribution' button.  Select your new S3 bucket.  

<img src="http://media.tumblr.com/tumblr_l8pwrkWndF1qzqijq.png" />

You can (optionally) have your Cloudfront distribution resolve via a CNAME.  If you're a GoDaddy.com customer and you'd like help creating a CNAME, <a href="http://community.godaddy.com/help/article/666">check out this help article</a>.

	<!-- When using the hostname provided by Cloudfront, you can reference the script like this -->
	<script src="http://d3h5p4zf44gxh.cloudfront.net/scripts/myscript.js"></script>

	<!-- When using a CNAME in Amazon Cloudfront, you can reference the script like this -->
	<script src="http://cdn.mydomain.net/scripts/myscript.js"></script>

