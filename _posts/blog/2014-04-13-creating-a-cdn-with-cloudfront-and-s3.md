---
layout: post
title: "Creating a CDN with Cloudfront and S3"
categories:
- blog
lede: "Because Cloudfront allows you to do some very complicated things the <a href='https://aws.amazon.com/documentation/cloudfront/'>official documentation</a> is very dense. I wanted to use it as a simple CDN for my website, and digging through the documentation to find what I needed was a bit of a chore."
hero: clouds.jpg
endnotes:
- "The image on this page is of nacreous clouds over Nasa's McMurdo Station in Antarctica (sourced from <a href='http://commons.wikimedia.org/wiki/File:Nacreous_clouds_Antarctica.jpg'>Wikimedia Commons</a>)"
---

What I really needed was a simple set of instructions. Since I couldn't find one when I was setting it up I've written a short guide for anyone who finds themselves in the same boat as me. 

## What are S3 and Cloudfront?

S3 and Cloudfront are both part of [Amazon Web Services](http://aws.amazon.com/) (AWS). S3, which stands for [Simple Storage Service](http://aws.amazon.com/s3/), is a cloud storage service. [Cloudfront](http://aws.amazon.com/cloudfront/) is a distributed content delivery network, which means that content is cached on multiple servers and served to users from the one closest to their location. Used together they allow you to create an efficient and potentially very cheap way of delivering content to your website. Access is metered and there is no subscription charge, so the price you pay depends on how much data you send and receive.

## Why use a CDN?

The main reason is to improve performance; using a CDN to serve static content offers significant performance improvements. In my case there was also a second reason: my website is hosted and served from Github and I didn't want to keep a large archive of media files in my website's Github repository. Using a CDN helped me keep things tidy. On the other hand, I did want my SASS, CSS and JavaScript files held in version control which is why I only use my CDN for image files.  

## What does this guide cover?

All I did was set up a simple CDN to serve image files to my website, and that's all that this guide covers. That said, once everything's been set up you can easily use your CDN to serve any other kind of static content that you want to, such as CSS or JavaScript.

One of the biggest problems I had was working out the steps I needed to go through to set everything up, but that turned out to be quite simple in the end. There are five things you need to do to get up and running:

1. Create an account with Amazon Web Services.
2. Create a bucket (Amazon terminology for a directory) to hold your image files. This is done in S3.
3. Create a Cloudfront distribution to serve the image files.
4. Specify a URL for your CDN and create a matching CNAME entry in your DNS records.
5. Prevent the files from being accessed at their original location.

## Creating an account with Amazon Web Services

This bit's easy. Go to [http://aws.amazon.com/](http://aws.amazon.com/) and either create an account or add AWS to an existing account. How you organise your life is up to you, but I like to keep work and impulse purchases separate so I have a separate account for AWS. I also turned on two-factor authentication. Unlike two-factor authentication on Github or Google, AWS requires you to enter an authorisation code every time you log in which is a bit more secure and a bit more annoying.

### Authorising your account

Amazon requires you to confirm your identity when you sign up for AWS by calling you and asking you to enter a pin. Despite there being nothing to suggest that you can't use a UK mobile number, in my case it didn't work. You only get three tries at confirming your identity before your account is locked for twelve hours, which is something you only find out after your third failed attempt. Very annoying. Use a landline if you can.

## Creating and configuring a bucket

* Log onto the [AWS console](https://console.aws.amazon.com/console/home) and click on the link to S3.
* Click on the create bucket name, and choose a unique name for your bucket. There are a lot of naming conventions for S3 buckets, but to keep things simple I gave mine the same name as my website, jea.tt.
* Choose a location for your bucket. There are lots of options, all of which affect the price, but the easiest thing to do is choose the region closest to where you live.
* Ignore the Set up Logging option for now (we'll come back to that at the end) and press the Create button.

Once you've done that, you'll see a link to your bucket in the All Buckets list on the left-hand side of the console. Click the link, and then set up your folder structure. How you do this depends on your project but in my case I used the same structure as I would have done if I was keeping the images with the rest of my website.

Once you've done that you should upload some test images. At this point you won't be able to see them, and that's the way it should be. Bucket policies&#8212;file permissions, basically&#8212;are one of the most complicated parts of S3, but in this case Cloudfront will do all that for you.


## Create a Cloudfront distribution

The next thing to do is create a distribution and hook it up to your bucket. If you click on the Services menu you'll find a link to Cloudfront.

* Click on the Create Distribution button.
* Choose web (or rather don't choose RTMP since web is selected by default) and click on continue.
* In the origin settings, choose your bucket as your origin domain name (you'll get a drop down list).
* Your origin ID will be automatically populated, and unless you have a good reason to change it I would leave it as it is.
* Ignore everything under Default Cache Behaviour Settings.
* Under Distribution Settings, choose the option which suits you best. Unless you know where all your traffic is coming from, or you're expecting there to be a lot of data transfer, then it's better to stick with All Edge Locations.
* Decide what you want your CDN to be called, and put it into the Alternate Domain Names field. This should be a subdomain of your website, which in my case was cdn.jea.tt. You don't need to include the http:// prefix.
* Ignore everything else and click create distribution.

## Add a CNAME record

After you've done that you'll need to add a CNAME record wherever your DNS is managed from. I use [DNSimple](https://dnsimple.com) but if you're on shared hosting then you should be able to do this from your control panel. 

What you're doing is setting up a sub-domain and then pointing it to your Cloudfront distribution. You may need to check with your host for specific instructions, but there are two parts to the record. The first, which is usually called Name, is the URL you added to the Alternate Domains Field (cdn.jea.tt in my case). The second, which I've seen called Destination, Content or just CNAME, is the domain name which Cloudfront has assigned to your distribution. To find it, open up the Cloudfront page of the AWS console and you'll see it listed in a table with all the other information about your distribution. It should look like a string of characters followed by cloudfront.net.

DNS changes can take a while to make their way around the web, but there are still a couple of things you can do while you're waiting. 

## Prevent the files from being accessed at their original location.

To stop your files being accessed without the CDN URL, you need to set up and configure an Origin Access Identity. This is the part of the process which caused me the most problems, because Origin Access Identity didn't sound like the thing I was trying to do so I ignored it. Once I worked it all out it was so easy that I shudder when I remember just how many hours I wasted getting it working. Here's what you do:

* Click on Cloudfront from the Services menu.
* In the left hand menu, there's a Private Content sub-menu. From there, click on  Origin Access Identity.
* From the Origin Access Identity screen click on the Create Origin Access Identity button.
* The comment field should already be populated, so all you need to do is click on the Create button.
* Click on Distribution from the left hand menu, then select your distribution and click on the Distribution Settings button. 
* From the Origins tab, select your domain name and click on the Edit button.
* Set Restrict Bucket Access to Yes. This should reveal some more options.
* Select Use an Existing Identity.
* Choose your identity from the select menu. There should only be one.
* Next to Grant Read Permissions on Bucket, select Yes, Update Bucket Policy.
* Click the Yes, Edit button.

You now need to check that the Bucket Policy settings are in place.

* Select S3 from the Services Menu.
* Click on your bucket, and then on the Properties button in the top right.
* Expand the Permissions menu. You should see an Edit Bucket Policy option. 
* If you click on that option, you'll see a policy with an ID of 'PolicyForCloudFrontPrivateContent'.
* As long as that's there then everything is set up properly so you can press the Close button. 

## Patience

It can take a few hours, or even a couple of days, for things to settle down. Nothing at all will work until your DNS changes have taken effect, and it can take several hours for the Origin Access Identity to start working as well. Even after I'd set everything up my CDN URLS were still redirecting to the S3 URLs, and I spent ages trying to work out why. And then, after writing a very long and probably very boring post on the AWS forums, it all started working. Luckily that happened before I hit submit, so I wasn't forced into an embarrassing public climbdown (only a private one.) 

## Testing

Remember those test images you uploaded? Once everything's been set up you should be able to access them using the CDN URL, which will be along the lines of http://cdn.yourdomain.com/path/to/images/image.jpg. Until everything has settled down this URL will redirect to your full bucket URL.

To test that the Origin Access Identity is working, try accessing the images using your bucket origin. You can find this on the Cloudfront console, under the Origin heading. All you need to do to test it is replace your CDN subdomain with the URL underneath the Origin heading. If everything's working properly, you should see an XML file with an Access Denied error code.

And that's it. Your CDN is up and running. You can now use the images in your HTML files by specifying the full path in the 'src' attribute. There are just two more things to take care of, only one of which is important, and then you're done.

## Cache busting

This is a big thing with CDNs and the final thing you need to worry about. Because your content is cached you can't just overwrite a file and expect the changes to show up. There are lots of ways to deal with cache busting, most of which I don't understand, but as this guide is about making life easy I would recommend using versioned file names which is every bit as low-tech as it sounds. If you have a picture of you called 'me.jpg' and you want to replace it with one in which you're looking a little bit sexier, upload 'me-1.jpg' or similar and change your file path. You should also delete the old one at the same time, just to keep things tidy.

## Logging

Whether or not you set up logging depends on whether or not you need it. I set it up because I was interested, but I haven't really used it. It's very easy to do though.

* Go to S3 and create a bucket to hold your logs. I called mine logs.jea.tt.
* Go back to Cloudfront, select your distribution, and then click on the Distribution Settings button.
* From the General tab, click the Edit button.
* Select On next to Logging.
* From the select menu choose the bucket you just created for your logs.
* Choose a prefix for your logs, including a slash at the end. Mine was cdn.jea.tt-logs/.
* You can leave Cookie Logging off.
* Click the Yes, Edit button.

A log file will now be generated every time a file is served from your CDN.