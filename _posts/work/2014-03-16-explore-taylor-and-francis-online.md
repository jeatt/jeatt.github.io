---
layout: project
title: "Explore Taylor &amp; Francis Online"
categories:
- work
lede: "I designed and built a website which gave the Taylor &amp; Francis marketing team the freedom to create and manage their own campaigns without having to rely on external designers or the in-house web team."
hero: explore-tfo-hero_v3.jpg
responsibilities:
- Research and planning
- Design and development
- Training
- Maintenance and support
---

When I started working for Taylor &amp; Francis (who publish academic journals) in 2008, 70% of our marketing was print. By 2012 70% of our marketing was online but most of it was done in a way which could be described as print, but on the web. The marketers would commission a designer to put together flyers, postcards or catalogues which would then be delivered as PDF files and uploaded to our website. 

Each piece of work had to go from the marketer to the designer and then back to the marketer before being passed on to the web team and uploaded to the website. This made it difficult to put together anything other than simple campaigns and it wasn't unusual for larger campaigns to be made up of a series of linked PDFs which offered a very poor user experience. It was also hard to measure the success of each campaign since Google Analytics doesn't track traffic to and from PDF documents very well. 

It wasn't a very efficient way of doing things, and my job was to come up with something better.

## Cutting out the middle men

I started by talking to the marketers and it quickly became obvious that what they wanted was to be able to do their work without having to rely on external designers or the web team. Cutting out the middle men would allow them to work faster, give them the freedom to flex their creative muscles and let them put together the campaigns they wanted.

The challenge was coming up with a system which flexible enough to let them do this and robust enough to make sure that the standard of the pages they were putting together was consistently high. It also had to be usable by people with a range of technical skills.

## Under the hood

During the research stage of the project I had noticed that most campaigns were made up of a small number of self-contained content types, such as article lists or conference announcements. Instead of giving users a way to create individual pages I used ExpressionEngine to create a content management system which kept the content and the pages separate.

The CMS allows the marketers to create the content they need for each campaign, and then drag and drop it onto the campaign pages in any combination and in whatever order they want. Content can be reused and shared across multiple pages, which is something which is used extensively for social media profiles and shared promotions. Although the majority of pages are standalone campaigns, it is also very easy to create multi-page campaigns.
![Article list and boxout](http://cdn.jea.tt/img/work/explore-tfo-article-list.jpg)

Because the layout of each content type is the same on every page, each campaign looks consistent. Applying universal updates is easily achieved and the site has already had one significant update to its layout. Because of the modular way in which the CMS has been set up, new content types can be created as and when they are needed with only minor changes having to be made to the page templates.

## Integration

The focus of marketing campaigns is often individual journals, so the marketers needed a way to associate a campaign page with a particular journal. Rather than adding this information to the site manually this is pulled into the site from our existing journals database. This makes adding journal information to a campaign page as simple as choosing it from a drop-down list. Design elements such as boxouts and journal images are then automatically added to the appropriate place on the campaign page, and the information about each journal is always up-to-date.
![Call for papers](http://cdn.jea.tt/img/work/explore-cfp.jpg)

## Keeping it in the family

The majority of marketing campaigns are designed to direct users to Taylor &amp; Francis Online, which hosts all of our online content. Explore Taylor &amp; Francis Online needed to look similar but it also had to improve on the overall user experience. I decided to keep certain design elements&#8212;the colour scheme, the logo, and a distinctive background gradient&#8212;but improve the typography and create a responsive layout to make better use of the available space and make sure that the pages would work on mobile devices. The end result was a website which had was identifiably part of the same company, but with an improved user experience.

Because the campaign pages are designed to be promoted individually the site doesn't have a home page or a main navigation, although it was designed with expansion in mind so a home page and top level team or subject pages can be created and made navigable if this ever becomes a requirement. 

The site was also designed to be used for our other imprints if necessary. A sister site has already been launched for Cogent OA, our Open Access imprint.
![Cogent OA](http://cdn.jea.tt/img/work/explore-cogentoa-detail.jpg)
