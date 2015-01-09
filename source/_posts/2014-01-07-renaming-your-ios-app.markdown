---
title: Renaming Your iOS App
date: 2014-01-07 00:00 UTC
layout: post
tags:
---

## Why

I recently released [Texties](http://www.mohawkapps.com/app/textables/) - a fun way to use unicode artwork and generate weird and upside down translations of text. I thought it was a really fun app and so did a lot of my friends. However, once it got into the app store, I was seeing a measly 1 download every 2-3 days. How on earth could this happen? I set out (along with some support from my friend [Andrew Gertig](http://www.andrewgertig.com/)) to figure out what was going on.

The first thing I did was a search for “texties” on my iPhone. 2200 search results including “InstaCollage”, Facebook Messenger, Facebook, and lots of photo apps. Shouldn’t an exact text search of the name of the app show that app first on the list?

Apparently, no. [Sam Soffes’s](http://soff.es/) app [Shares](http://getsharesapp.com/) shows up 11th on a search for “shares” (also with 2200 results) despite being featured by Apple in the finance category.

Sam [tweeted me](https://twitter.com/soffes/status/417753933223059456) and got me thinking that Apple is doing some really strange text autocorrection to search terms. It seems that an App Store search for “txts” results in the same exact results in the same exact order as “texties”. ==Seriously, Apple?!==

It seemed that a name change was in order. I thought *Texties* was a really great name. It was fun, whimsical, rhymed with testes (thanks [Adam](https://twitter.com/ahow)), and could be used singularly to describe an individual *Textie* that you would send to people.

Ultimately, I settled on the new name: *Textables*. Even though it’s a bit longer and doesn’t rhyme with male genetalia, it still had the ability to be singularized. Most importantly, there were only three search results for the exact word! ==Eureka!==

## How

Renaming an app is no simple task. You don’t realize how many places you put the app name:

* iTunes Connect metadata
* Your personal/company site
* Inside carefully crafted screenshots
* Github repositories
* Social media
* Within the app itself


==iTunes Connect== is pretty easy. Just create a new version of the app and edit the metadata, changing all the instances of the original name over to the new name. Don’t forget about the keywords and description.

I’ve got my own company ==website== where I promote my apps but I had purchased textiesapp.com to use as a forwarding url to the product page on my site. Looks like that’s $10 I’ll never recoup the costs on. I decided not to purchase a vanity domain for Textables so I simply changed the app name on my site and the permalink (making sure to forward the old Texties url to the new Textables url) and updated the forwarding of the vanity URL.

The ==app screenshots== were the most difficult and time consuming thing to change. This was the first app where I had used [an image of an iPhone in the screenshots](https://github.com/MohawkApps/Textables/#screenshots) with text overlaid on the image. So not only did I have to re-take the screenshots, I also had to do a bunch of text editing in order to change the text in the images and reposition the new screenshots in the images. This took me a few hours to complete.

Changing your ==github repository== name is really easy. Just rename it and github forwards all traffic from the old url to the new url. I had to spend some time and update the `Readme.md` file to reference the new name.

I had only set up one ==social media== account for Texties. Changing @TextiesApp to [@TextablesApp](https://twitter.com/textablesapp) was pretty straightforward in the Twitter account preferences.

Changing the app’s name in ==the app itself== would probably sound like the most time consuming part of this whole process, but it wasn’t. I’m using [RubyMotion](http://www.rubymotion.com/) for Textables and with a simple change of the `app.name` in the `Rakefile` and a change of the folder name in my working directory, the app was completely renamed. The word “Texties” shows up in the app no less than 10 times, but I use [BubbleWrap’s](http://www.bubblewrap.io) `App.name` shortcut everywhere. I never hardcoded “Texties” into the app anywhere except in the `Rakefile`. *Neat, huh?*

So off I went to complete the rest of the deploy procedure:

1. Increment version numbers in the project


2. Mark app version as binary ready to upload in iTC


3. Run `rake archive:distribution` to create the files


4. Copy the `build` folder over to my `releases` folder (I keep binaries and dsyms for every release)


5. Use `Application Loader.app` to submit the binary to Apple


6. …


7. Profit!


Keeping my code [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself) and setting up the code to use text variables instead of hardcoding text all over the place made it extremely easy to change the name of my app in the app itself. Thanks to RubyMotion and Bubblewrap, all I really had to do was change the variable in the `Rakefile`. the majority of the time was spent updating metadata and images.

So hopefully this new name change will have a net positive effect on Textables.

[Check it out on the App Store for $0.99!](https://itunes.apple.com/us/app/textables-unicode-ascii-art/id769404785?mt=8&uo=4&at=10l4yY&ct=blog-renaming)
