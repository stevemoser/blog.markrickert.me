---
title: Crap! An Xcode Update Wiped Out My iOS 6 SDK Again!
date: 2013-11-14 00:00 UTC
layout: post
tags:
---

If you're an iOS developer that works on lots of varied client projects, you have probably tried (unsuccessfully, like me) to manage multiple SDK versions in Xcode.

Unfortunately, I have clients that are still trying to release software with the iOS 6 SDK. Sometimes it's just not worth it to update the entire application to iOS 7 just for a little bug fix, but most of the time I have to deploy using the iOS 6 SDK simply because the client doesn't want to take the time and money to update the entire look and feel of their application. ==Business implications of this decision aside==, I recommend what I think I should to do the client, but ultimately I need to implement what they want. Yes, anecdotal evidence suggests that Apple accepts submissions using the previous generation SDK.

This leads to a problem: ==keeping old versions of SDKs around==. I used to just throw the old SDK version inside the *Xcode.app* and call it a day. But then if I forgot to keep that ~1gb folder around when I hit that "Update" button in the App Store app, the new version of Xcode would erase my old SDK folders. This was a pain in the ass to remember each time I did an update to Xcode. Cue logging in to the developer center, finding the old version of Xcode, downloading the gigantic *.dmg* file, and copying the SDK back into Xcode.

I searched online to try and figure out how other people dealt with this problem and it seems like the [overwhelming sentiment](http://stackoverflow.com/a/18424373/814123) is to copy the SDKs out of *Xcode.app* and symlink them back. That *SO* link has a link to a [gist written in python](https://gist.github.com/rnapier/3370649) that was supposed to do this for me, but it didn't work, so I decided to write my own utility in Ruby to manage this process for me.

Essentially, you can run this as soon as you update Xcode and it will preserve your old SDK files, copy any new or current SDK files into an */SDKs* folder on your computer and then symlink them all back into Xcode... ==even your old one that this version of Xcode didn't come with==.

Here's the script. All you have to do is create the */SDKs* folder, and run this script (with `sudo`, and don't forget to `chmod +x` the file). It's friendly and will tell you what it is doing every step of the way!

```ruby
#!/usr/bin/env ruby

# fix-xcode
#   Mark Rickert <mjar81@gmail.com>

# Symlinks all your old SDKs to Xcode.app every time it is updated.
# Create a directory called /SDKs and run this script.
#
# Each time you upgrade Xcode, run fix-xcode.

require 'FileUtils'

def main

  # Find all the SDKs in Xcode.app that aren't symlinks.
  Dir.glob("#{xcode_path}/Platforms/*.platform/Developer/SDKs/*.sdk") do |sdk|
    basename = sdk.split('/').last
    if File.symlink?(sdk)
      puts "#{basename} is already symlinked... skipping.\n"
      next
    end

    puts "Processing: #{basename}\n"

    # Remove the old version if it exists
    destination = "#{sdk_path}/#{basename}"
    if File.directory?(destination)
      puts " - Removing existing SDK: #{destination}.\n"
      FileUtils.rm_rf destination
    end

    puts " - Moving the Xcode version into place in #{sdk_path}.\n"
    FileUtils.mv sdk, sdk_path
  end

  Dir.glob("#{sdk_path}/*.sdk") do |sdk|
    sdk_name = sdk.split("/").last
    sdk_platform = sdk_name.match(/[a-zA-Z]{3,}/)[0]

    ln_dest = "#{xcode_path}/Platforms/#{sdk_platform}.platform/Developer/SDKs/#{sdk_name}"
    puts " - Symlinking #{sdk_platform}.\n"

    FileUtils.ln_sf sdk, ln_dest
  end

  puts "\nDone! Your SDKs now live in #{sdk_path} and are symlinked properly into the Xcode.app.\n\n"
end

def xcode_path
  `xcode-select --print-path`.chomp
end

def sdk_path
  "/SDKs"
end

def remove_sdk version
  # Remove the iOS SDK from xcode.
  version = version.to_s << ".0" if version.to_s.length == 1

  puts "-" * 29
  puts "Removing the iOS #{version} SDK from the Xcode Bundle."
  puts "-" * 29

  removing = "#{xcode_path}/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS#{version}.sdk"
  if File.exist? removing
    FileUtils.rm_rf removing
    puts "SDK successfully removed. Please restart Xcode."
  else
    puts "Couldn't find that SDK at path: #{removing}"
  end

end

if ARGV.count == 2
  if ARGV[0] == "remove"
    abort "Please pass another parameter specifying what SDK to remove." unless ARGV[1]
    remove_sdk "#{ARGV[1]}"
  end
else
  puts "-" * 29
  puts "Running Fixing Xcode.app SDK Paths."
  puts "-" * 29

  main
end
```

Here's a link to the Gist: https://gist.github.com/markrickert/7459455
