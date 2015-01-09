---
title: Prepending the Default RubyMotion Build Task
date: 2014-09-18 00:00 UTC
layout: post
tags:
---

==UPDATE - I've rewritten this article with a more elegant solution.==

Thank's to the helpful Hipbyte team member [Watson1978](https://github.com/Watson1978), I was able to [figure out how](https://github.com/HipByte/RubyMotion/pull/171) to run tasks before and after build processes in RubyMotion. I've ofeen wanted to do things before building to the simulator like downloading assets into the `Resources` folder, but never knew how to do it. My solution was to extend the core RM build processes and alias methods, override the methods, and then run what I wanted to before running the aliased methods... yeah... annoying, huh?

It's actually really easy to do with a little know-how and the [rake-hooks](https://github.com/guillermo/rake-hooks) gem!

First, add `gem 'rake-hooks'` to your `Gemfile` and run `bundle`.

Then all you have to do is use `before` and after` blocks in your `Rakefile` like this:

```ruby
before :"build:simulator"
  # File and path definitions
  file_path = 'resources/file_i_want.json'
  web_path = 'http://someurl.com/file_i_want.json'

  # Check to see if the file exists
  unless File.exist?(file_path)
    # If not, download it and put into the resources directory
    require 'open-uri'
    open(file_path, 'wb') do |file|
      file << open(web_path).read
    end
  end
end

after :clean do
  puts "Deleting resource file."
  file_path = 'resources/file_i_want.json'
  File.delete(file_path) if File.exist?(file_path)
end
```

You can define `before` and `after` for any of the RubyMotion rake tasks. To see a list of tasks, just run `rake -T` but the most common ones will be:

```bash
rake build:device
rake build:simulator
rake spec:simulator
rake clean
rake archive:distribution
```

I hope this little tip helps you automate your rake tasks in RubyMotion!
