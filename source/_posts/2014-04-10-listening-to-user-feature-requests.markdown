---
title: Listening to User Feature Requests
date: 2014-04-10 00:00 UTC
layout: post
tags:
---

I've gotten a lot of positive feedback on my new iOS app [aloft](http://www.mohawkapps.com/app/aloft/). One user emailed me and asked if I could make the weather station screen searchable. I told him "no problem, it'll be in the next version that ships".

Searchable tableviews are not hard to implement (more on that in a bit), but I had left it out simply for the fact that I didn't think users would need that feature. The app sorts the weather stations based on closeness to your device and ==I incorrectly assumed== that users would just want to use the closest weather station.

So I got to work. Luckily I'm using the [ProMotion](https://github.com/clearsightstudio/ProMotion/) RubyMotion framework and making a `ProMotion::TableScreen` searchable is not just easy--it's trivial. It's [literally one line of code](https://github.com/clearsightstudio/ProMotion/wiki/API-Reference:-ProMotion::TableScreen#searchableplaceholder-placeholder-text).

I wanted the search to match more than just the title and subtitle of the table cells, so I added the `search_text:` attribute to my table cell creation method and added the text that I wanted to be searchable.

> [Here's the commit](https://github.com/MohawkApps/aloft/commit/079230badc0135171634c97e5e65913550c77a30)

Overall, this feature took me 2 lines of code to implement using ProMotion. How awesome is that? If you haven't checked out ProMotion yet, give [the wiki](https://github.com/clearsightstudio/ProMotion/wiki) a glance and see if maybe you could use it in your next RubyMotion project.

Lesson learned? Listen to your users. Sometimes what you think they are going to want isn't actually what they want.

<small><em><strong>Full Disclosure:</strong> I am a core contributor on the ProMotion project.</em></small>
