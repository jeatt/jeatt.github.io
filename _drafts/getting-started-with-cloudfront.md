---
layout: post
title: "Setting up a simple CDN with Cloudfront and S3"
categories:
- blog
lede: "Cloudfront allows you to do some very complicated things and the documentation reflects that. I wanted to do something simple, which meant sifting through all of those words trying to find the ones which were relevant to me."
hero: clouds.jpg
endnotes:
- "The image on this page is of nacreous clouds over Nasa's McMurdo Station in Antarctica (sourced from <a href='http://commons.wikimedia.org/wiki/File:Nacreous_clouds_Antarctica.jpg'>Wikimedia Commons</a>)"
---

It wasn't much fun, so I've written up this short guide to hopefully spare you from the frustration that I went through.

## What are S3 and Cloudfront?

S3 and Cloudfront are both part of [Amazon Web Services](http://aws.amazon.com/) (AWS). S3, which stands for [Simple Storage Service](http://aws.amazon.com/s3/), is a cloud storage service; [Cloudfront](http://aws.amazon.com/cloudfront/) is a content delivery network. When used together they allow you to create an efficient and potentially very cheap way of delivering content to your website. Access is metered and there is no subscription charge, so the price you pay depends on how much data you send and receive.

## What was I trying to do with it?

All I wanted to do was to create a CDN to serve image files to my website, which is hosted on website on Github. It wasn't really about performance, although improved performance is nice, but more to do with the fact that I wanted to keep my repository nice and tidy. 

That's the short answer. When I started, I didn't really know what the long answer was and that's where things started to go wrong. Since I didn't really know what I was trying to do I ended up stumbling around the web hoovering up mismatched pieces of information like a tipsy guest at a wedding buffet.

The long answer turned out to be quite short really, and in the same way that all the tipsy guest really needs is a sandwich, a handful of crisps, a glass of water and somewhere to sit there were only four things that I needed:

1. A directory (a bucket in Amazon terminology) to hold my image files. 
2. A Cloudfront distribution to serve the image files.
3. A URL for my CDN and a matching CNAME entry in my DNS records.
4. A way to prevent the files from being accessed at their original location.

## Create an account with Amazon Web Services

This bit's easy. Go to [http://aws.amazon.com/](http://aws.amazon.com/) and either create an account, or add AWS to an existing account. How you organise your life is up to you, but I like to keep work and impulse purchases separate so I have a separate account for AWS. I also turned on two-factor authentication. Unlike two-factor authentication on Github or Google, AWS requires you to enter an authorisation code every time you log in which is both a bit more secure and a bit more annoying.

### Authorising your account

Amazon requires you to confirm your identity when you sign up for AWS by calling you and asking you to enter a pin. Despite there being nothing to suggest that you can't use a UK mobile number, in my case it didn't work. You only get three tries at confirming your identity before your account is locked for twelve hours, which is something you only find out after your third failed attempt. Use a landline if you can.

## Create and configure a bucket

* Log onto the AWS console and click on the link to S3
* Click on the create bucket name, and choose a name for your bucket. There are a lot of naming conventions for S3 buckets, but to keep things simple I gave mine the same name as my website: jea.tt.
* Choose a location for you bucket. There are lots of options, all of which affect the price, but the easiest thing to do is choose the region closest to where you live.
* Ignore the 'set up logging' option for now (we'll come to that later) and press the 'Create' button.

Once you've done that, you'll see a link to your bucket in the All buckets list on the left-hand side of the console. Click the link, and then set up your folder structure. How you do this depends on the project of course, but in my case I used the same structure as I would have done if I was keeping the images with the rest of my website.

Once you've done that you should upload some test images. At this point you won't be able to see them, and that's the way it should be. Bucket policies&#8212;file permissions, basically&#8212;are one of the most complicated parts of S3, but in this case Cloudfront will do all that for you.


### Create a Cloudfront distribution

The next thing to do is create a distribution and hook it up to your bucket. If you click on the services, you'll find a link to Cloudfront.

* Click on the Create Distribution button.
* Choose web (or rather don't choose RTMP since web is selected by default) and click on continue.
* In the origin settings, choose your bucket as your origin domain name (you'll get a drop down list).
* Your origin ID will be automatically populated, and unless you have a good reason to change it I would leave it as it is.
* Ignore everything under Default Cache Behaviour Settings.
* Under Distribution Settings, choose the option which suits you best. Unless you know where all your traffic is coming from, or you're expecting there to be a lot of data transfer, then it's better to stick with All Edge Locations.
* Decide what you want your CDN to be called, and put it into the Alternate Domain Names field. Usually this is a subdomain of your website, which in my case was http://cdn.jea.tt.
* Ignore everything else and click create distribution.

### Add a CNAME record

After you've done that you'll need to add a CNAME record wherever your DNS is managed from. I use [DNSimple](https://dnsimple.com) but if you're on shared hosting then you should be able to do this from your control panel. 

What you're doing is setting up a sub-domain and then pointing it to your Cloudfront distribution. You may need to check with your host for specific instructions, but there are two parts to the record. The first, which is usually called Name, is the URL you added to the Alternate Domains Field (cdn.jea.tt in my case). The second, which I've seen called Destination, Content or just CNAME, is the Domain Name which Cloudfront has assigned to your distribution. To find it, open up the Cloudfront page of the AWS console and you'll see it listed in a table with all the other information about your distribution. It should look like a string of characters followed by cloudfront.net.

DNS changes can take a while to make their way around the web, but there are still a couple of things you can do while you're waiting. 

### Origin Access Identity

Origin Access Identity is how you stop people accessing your files without using your CDN URL. This is where all the pain and frustration came in, but it's actually really easy as long as you can find a good explanation (like this one, hopefully).

Origin Access Identity didn't sound like the thing I needed to do so I ignored it for ages. Once I worked it all out it was so easy that I shudder when I remember just how many hours I wasted getting it working. Here's what you do:

* Click on 'Create Origin Access Identity'
* 

## Patience

It can take a few hours, or even a couple of days, for things to settle down. Nothing at all will work until your DNS changes have taken effect, and it can take several hours for the Origin Access Identity to start working as well. Even after I'd set everything up my CDN URLS were still redirecting to the S3 URLs, and I spent ages trying to work out why. And then, after writing a very long and probably very boring post on the AWS forums, it all started working. Luckily before I hit submit, so I wasn't forced into an embarassing public climb down (only a private one.) 

## Cache busting

This is a big thing with CDNs and the final thing you need to worry about. Because your content is cached you can't just overwite a file and expect the changes to show up. There are lots of ways to deal with cache busting, most of which I don't understand, but as this guide is about making your life easy I would recommend using versioned file names which is every bit as low-tech as it sounds. If you have a picture of you called 'me.jpg' and you want to replace it with one in which you're looking a little bit sexier, upload 'me-1.jpg' and change your file path. You should also delete the old ones at the same time, just to keep things tidy.

That's it.




