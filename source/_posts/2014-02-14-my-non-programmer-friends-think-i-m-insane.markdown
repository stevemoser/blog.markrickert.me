---
title: My Non-programmer Friends Think I'm Insane
date: 2014-02-14 00:00 UTC
layout: post
tags:
---

North Carolina has been hit with a [massive snow storm](http://www.newsobserver.com/2014/02/12/3614577/snow-sleet-and-freezing-rain-for.html) over the past few days. The word “massive” doesn’t do it justice-Charlotte has been crippled for 2+ days now. Busses just started running again today.

Luckily I live in a very walkable neighborhood and lots of friends live close by. So we took the opportunity to get some fresh air and walk (which at that point was safer than driving). Cars were going by very slowly as we walked down the invisible sidewalk. 

Then “the showoff” drive by. He (I’m assuming it was a he) was driving a big truck… the kind of truck that nobody needs in a city but they drive one anyway in a feeble attempt to impress people that don’t really care. He guns it and the back tires start spinning wildly, the back end fishtailing violently back and forth.

He slowed down and did it again. One of my friends says, “I hope they get home OK.” I nod in agreement.

I proceed to say, “If he wrecks, I hope it happens close to us.” Not because I wanted to watch the carnage, but because there was ==nobody== around and we could have called 911 or assisted them somehow.

My friends were aghast that I would even _think_ about a car crash happening to someone, even if they were clearly being an idiot.

**That got me contemplating the thought process that went on in my head to verbalize that statement.**

As a software developer, I’ve trained my brain to assess a situation and attempt to handle all possible scenarios. And it’s always the bad scenarios where you have to do the most work… error handling, figuring out what to do if you get bad data back from the server or user, etc.

Essentially in my head, I wrote this code:

```ruby
truck = Truck.new(size: 'compensating for something')

while truck.drive do
  if people_watching
    truck.showoff('spin wheels')
  end
end

class Truck < Automobile
  attr_accessor :distance
  def drive
   @distance++
  end

  def showoff(how)
    fishtail if how == 'spin wheels'
  end

  def fishtail
    if [1..10].to_a.sample == 1
     massive_crash
    else
     5.times do
       drive
     end
    end
  end

  def massive_crash
    # TODO: How to handle this error?
  end
end
```

It was a non-event if everything was OK. Handling the error was the thing my brain went to first. We already know the truck is driving forward and fishtailing… but that one time where the simplistic random number generator hits the wrong integer, ==ka-blam!== How do I handle the edge case?

My father had a saying when I was a kid:

> Intelligence is the ability to foresee what can happen by your actions before you do it.

I like to add “…as well as the actions of others.”

I’m pretty sure my non-programmer friends think I’m insane. I’m also pretty sure that people think I’m a pessimist because I try to figure out what I’ll do in bad scenarios before they happen. 

I think my software development career has affected my thought process in a positive way and changed the way I think and react to events in my everyday life. ==I’m not insane or pessimistic. I’m prepared.==
