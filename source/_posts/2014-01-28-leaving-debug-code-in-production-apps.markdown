---
title: Leaving Debug Code in Production Apps
date: 2014-01-28 00:00 UTC
layout: post
tags:
---

Typically, leaving debug code in production builds isn’t a great idea, but I do it all the time and so should you.

With a bit of configuration, ==it’s perfectly OK== to leave debug statements in your production code and it adds negligible overhead to your running application. Doing this benefits you by not having to remember to comment out all your debug code before building for production and you automatically get logs and debug info when you run locally or in development mode.

Lets talk about how to do this for a RubyMotion app!

I love using the [`motion_print`](https://github.com/OTGApps/motion_print) gem for my logging. It’s a drop-in replacement for `puts`. just change all your `puts` statements to `mp`.

Next, we need to have a way of telling the app that it is running in production mode, for that, we add a bit of code to the `Rakefile`. This will set a boolean in the `Info.plist` file that tells the app that it is an app store release mode, ==only when you compile in distribution mode==:

```ruby
Motion::Project::App.setup do |app|
  # ...
  app.release do
    app.info_plist['AppStoreRelease'] = true
    # ...
  end
end
```

Next, in your app delegate let the app know what to do if that key exists:


```ruby
class AppDelegate
  def application(application, didFinishLaunchingWithOptions:launchOptions)
    BW.debug = true unless App.info_plist['AppStoreRelease'] == true
    # ...
end
```

I’m using [BubbleWrap’s](http://www.bubblewrap.io) debug property to manage debug mode globally in my application. It’s an easy way to detect debug mode anywhere within your app and do things conditionally. It has the added benefit of automatically printing out HTTP headers if you’re using the `BW::HTTP` module.

From now on in the code, whenever you do an `ap` (or a `puts`, or whatever), just append it with `if BW.debug?` like so:

```ruby
def some_method(args={})
  # These lines won't do anything when
  # running in an App Store build:
  mp args if BW.debug?
  puts args if BW.debug?
  NSLog("%@", args) if BW.debug?
  call_some_other_method if BW.debug?
end
```

Maybe you could even extend the `motion_print` gem to automatically do this for you when you call the `mp` method ;)

I hope this little trick makes your debugging and release cycles easier! Hit me up on Twitter to tell me I’m wrong or that I’m handsome! [@markrickert](http://twitter.com/markrickert)
