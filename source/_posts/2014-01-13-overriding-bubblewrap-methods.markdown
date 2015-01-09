---
title: Overriding BubbleWrap Methods
date: 2014-01-13 00:00 UTC
layout: post
tags:
---

I found myself in need of overriding a core BubbleWrap method: `App.name`.

==Why?== you ask? Simple: my app name on the home screen is not the full title of my app. I wanted to be able to use the entire app name within the app itself using the `App.name` convenience method. ==Here’s how I did it.==

Rakefile:


```ruby
Motion::Project::App.setup do |app|
  app.name = 'Great App'
  app.info_plist['FULL_APP_NAME'] = 'My Great App'
  # ...snip
end
```

bubblewrap_overrides.rb:

```ruby
class BubbleWrap::App
  class << self
    # Use my name I specify in the app info plist.
    alias_method :short_name, :name
    def name
      App.info_plist['FULL_APP_NAME']
    end
  end
end
```

From now on, I can access the full app name using `App.name` and the “short” app name using `App.short_name`

Pretty cool, huh?
