---
title: Using Reveal with RubyMotion
date: 2013-10-15 00:00 UTC
layout: post
tags:
---

With today's release of [Reveal 1.0](http://revealapp.com/), I thought I'd highlight how easy it is to integrate into your RubyMotion project. If you don't know, Reveal is a neat tool that allows you to inspect almost anything about your iOS application in a 3D, exploded view.

According to their integration guide, you can use their `Reveal.framework` or a more complicated integrated of their dynamic library. Luckily, they've provided [a CocoaPod](https://github.com/CocoaPods/Specs/blob/master/Reveal-iOS-SDK/1.0.0/Reveal-iOS-SDK.podspec) for easy integration into your project and since RubyMotion supports CocoaPods integration by default, it's a snap to use Reveal!

First things first: we don't _ever_ want to ship the application with the Reveal framework in it, so we're going to add a second `app.pods` block inside the `app.development` block like this:

```ruby
app.development do
  app.pods do
    pod "Reveal-iOS-SDK"
  end
end
```

This will ensure that when you run `rake archive:distribution` to compile for the App Store that the Reveal framework will not be present in your app.

Then go to the commandline and run a few commands (these assume you're using the most recent version of `motion-cocoapods`):

```bash
bundle update
rake pod:install
rake
```

You should see your app compile and in the console you'll see something like `2013-10-15 09:16:50.121 YourApp[47334:a0b]  INFO: Reveal server started.` to let you know that the integration was successful.

Now fire up Reveal and you should see your app in the dropdown in the top left.

Have fun debugging with Reveal. Its a powerful tool and even has a 30-day free trial so you can check it out before you buy! I'm excited to see how this changes my development and debugging practices.
