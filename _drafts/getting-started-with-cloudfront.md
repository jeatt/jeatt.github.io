---
layout: post
title: "Getting started with Cloudfront"
categories:
- blog
summary: "he subject of what should, and shouldn't, go into version control is a contentious one. My opinion is that only images which are part of the site structure&#8212;logos, icons etc.&#8212;should go into the repository."
standfirst: "The subject of what should, and shouldn't, go into version control is a contentious one. My opinion is that only images which are part of the site structure&#8212;logos, icons etc.&#8212;should go into the repository. User uploaded content shouldn't. Because this website is hosted on Github and I wanted to keep my repository clean, I had to find somewhere else to put the images. I decided to go with Amazon S3 and Cloudfront. Setting it up was a horrible time for me."
hero: clouds.jpg
endnotes:
- "The image on this page is of nacreous clouds over Nasa's McMurdo Station in Antarctica (sourced from <a href='http://commons.wikimedia.org/wiki/File:Nacreous_clouds_Antarctica.jpg'>Wikimedia Commons</a>)"
# Lede is the first paragraph of an article. Standfirst is a summary. #
---



Configuring Cloudfront was a frustrating, harrowing, experience. It allows you to do some amazingly complex things and the documentation reflects that. I wanted to do something really simple and sifting through thousands of words trying to find the ones which were relevant to me was a deeply unpleasant way to spend a Saturday.

## What are S3 and Cloudfront?

S3 and Cloudfront are both part of Amazon Web Services (AWS). S3, which stands for Simple Storage Service, is a cloud storage service; Cloudfront is a content delivery network. When used together they allow you to create an efficient and potentially very cheap content delivery network (CDN). Access is metred and there is no subscription charge, so the price you pay depends on how much data you send and receivd over Amazon's network. 

## What was I trying to do with it?

Not having an answer to this question is where things started to go wrong for me. I didn't really know what I was trying to do which left me stumbling around the web hoovering up mismatched pieces of information like a drunken uncle at a wedding buffet. In much the same way as he only really needs a sandwich, a small handful of crisps, a glass of water and a nap, there were only four things I really needed to do.

1. Create a directory (a bucket in Amazon terminology) to hold my image files. 
2. Create a Cloudfront distribution to serve the image files.
3. Specify a URL for my CDN and add a matching CNAME entry to my DNS records.
4. Prevent the files from being accessed at their original location.

## Create an account with Amazon Web Services

This is easy. Go to [http://aws.amazon.com/](http://aws.amazon.com/) and either create an account, or add AWS to an existing account. How you organise your life is up to you, but I like to keep work and impulse purchases separate so I have a separate account for AWS. I also turned on two-factor authentication. Unlike two-factor authentication on Github or Google, AWS requires you to enter an authorisation code every time you log in. 

### Authorising your account

Amazon requires you to confirm your identity when you sign up for AWS by calling you and asking you to enter a pin. Despite there being nothing to suggest that you can't use a UK mobile number, in my case it didn't work. You only get three tries at confirming your identity before your account is locked for twelve hours, which is something you only find out after your third failed attempt. So use a landline if you have one.

### Create and configure a bucket

* Log onto the AWS console and click on the link to S3
* Click on the create bucket name, and choose a name for your bucket. There are a lot of naming conventions for S3 buckets, but to keep things simple I gave mine the same name as my website: jea.tt.
* Choose a location for you bucket. There are lots of options, all of which affect the price, but the easiest thing to do is choose the region closest to where you live.
* Ignore set up logging (we'll come to that later) and press the 'Create' button.

Once you've done that, you'll see a link to your bucket in the All buckets list on the left-hand side of the console. Click the link, and then set up your folder structure. In my case I was only hosting images so my folder structure looks like this:

-img
  -post
    -thumb
  -project
    -thumb

The next thing you should do is upload some test images. At this point you won't be able to see them, and that's the way it should be. Bucket policies&#8212;file permissions, basically&#8212;are one of the most complicated parts of S3, but in this case Cloudfront will do all that for you.

That's it for S3.


### Create a distribution

### Add a CNAME record

### Origin Access Identity

## Have some patience

## Alternatives

### Fastly

This looks great. Nice website, nice interface. One-click integration with DNSimple. Minimum charge of $50 a month. Bit too rich for my blood.

