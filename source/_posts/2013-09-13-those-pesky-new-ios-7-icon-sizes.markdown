---
title: Those pesky new iOS 7 Icon Sizes
date: 2013-09-13 00:00 UTC
layout: post
tags:
---

I went to deliver an app to the app store I'd been working on with updates for iOS 7 and got a nice little warning from the application loader that I needed to include new icon sizes for iOS 7. Oddly enough, iTunes Connect accepted the binary and I got an email that said I should look to include those files in my next release or invalidate the binary and resubmit.

It turns out that there are [a few new icon sizes](http://blog.manbolo.com/2013/08/15/new-metrics-for-ios-7-app-icons) for iOS 7 but RubyMotion doesn't automatically detect them and put them into the `Info.plist` file.

The iOS 6 SDK (and possibly iOS 7) will auto detect the icon files in your resources directory based on a predetermined file name. From everything I've read, the new image manager in Xcode 5 makes this functionality go away and iOS 7 just looks at the image sizes of the files listed in your `Info.plist` to determine what file to use on each device. I wasn't able to find anywhere that had the new "correct" file name documented for the 152px, 120px, and 76px icon sizes. It's most likely `icon-76@2x.png`, `icon-60@2x.png`, and `icon-76.png` (respectively), but I haven't tested those out. iOS 6 didn't need any icon files defined in order to use them in the app. 

Here's a quick one-liner to add to your [RubyMotion](http://www.rubymotion.com/) project `Rakefile` that will autodetect all the icons in your resources folder and add them to the `Info.plist` so you don't even have to worry about it (as long as you have all the right file sizes in there). Obviously, you'll want to change this to match your icon names and you can't have anything else in your `resources` directory named `icon*.png`.

```ruby
app.icons = Dir.glob("resources/icon*.png").map{|icon| icon.split("/").last}
```

Now when I run `rake config` I get

```bash
...snip...
icons : ["icon-120.png", "icon-152.png", "icon-72.png", "icon-72@2x.png", "icon-76.png", "icon-Small-50.png", "icon-Small-50@2x.png", "icon-Small.png", "icon-Small@2x.png", "icon.png", "icon@2x.png"]
...snip...
```

instead of

```bash
...snip...
icons : []
...snip...
```

Happy compiling!
