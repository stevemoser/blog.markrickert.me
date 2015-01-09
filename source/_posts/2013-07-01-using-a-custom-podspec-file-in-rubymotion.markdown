---
title: Using a Custom podspec file in RubyMotion
date: 2013-07-01 10:00 UTC
layout: post
tags:
---

I love using [CocoaPods](http://cocoapods.org) for my iOS applications. I also love that RubyMotion has CocoaPods support built right into the runtime.more All you have to do is add this to your `Gemfile`: 

```
#Cocoapods
gem "motion-cocoapods"
gem "cocoapods"
```

Then make sure your `Rakefile` looks like this:

```
# -*- coding: utf-8 -*-
$:.unshift("/Library/RubyMotion/lib")
require 'motion/project/template/ios'
require 'bundler'

Bundler.setup
Bundler.require

Motion::Project::App.setup do |app|
# Your application setup
# ...
```

Then you can include CocoaPods to your heart’s content in the `app.pods do` block. You can always specify the simple `pod "TestFlightSDK"` or `pod "AFNetworking"`. You can even specify the version you want with this syntax: `pod "Google-Maps-iOS-SDK", '~> 1.2`, but did you know that *you can specify your own custom podspec file* for projects that don’t have one in the main [specs repository](https://github.com/CocoaPods/Specs)?

Just create your `[SpecName].podspec` file and throw it somewhere in your project. I like `./vendor/custom_podspecs/`. Make sure the podspec is valid and passes all the CocoaPods tests (run `pod spec lint [SpecName].podspec`). Then just use this syntax:

```
app.pods do
  pod 'SpecName', :podspec => 'vendor/custom_podspecs/SpecName.podspec'
end

```

It’s as simple as that. You can do everything in the RubyMotion `Rakefile` that you can do in a regular `Podfile`. Here’s [the documentation](http://docs.cocoapods.org/podfile.html#pod) for the `Podfile` format. Go nuts and specify alternate git urls, different commits for the projects, or where the project is locally.
