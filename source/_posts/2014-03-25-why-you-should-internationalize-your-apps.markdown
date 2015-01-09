---
title: Why You Should Internationalize Your Apps
date: 2014-03-25 00:00 UTC
layout: post
tags:
---

> This post originally appeared in [Issue #20](http://rubymotiondispatch.com/issues/2014/issue-20/) of the [RubyMotion Dispatch](http://rubymotiondispatch.com/) weekly newsletter on March 25, 2014

Internationalization (`i18n`) and localization (`l10n`) of your apps are a great way to reach a wider audience. The App Store is a global marketplace, so why not try and cater to as many as possible?

You may be asking, "What's the difference between i18n and l10n?" I18n is the process of making your app localizeable. Theoretically you should only have to do this once. L10n is done multiple times, once for each locale you want to support.

The i18n process should include things like:

1. Abstracting all text into an external file located in your `/resources` folder. [Sugarcube has some abstractions](http://colinta.com/thoughts/rubymotion_i18n.html) that make this really easy.
2. Making sure things like dates, times, currency values, sort order, etc. are formatted according to the user's [`NSLocale.currentLocale`](http://nshipster.com/nslocale/)
3. Make sure to test languages other than your native tongue to see how they look. German words are on average _30% longer_ than English!Ensuring that very long text strings will fit and look good without truncation might take some time. Use `UILabel`'s `sizeToFit` method or for even easier global styling, use RMQ's [UILabelStyler](https://github.com/infinitered/rmq#uilabelstyler),

Once you've internationalized your app, you do multiple localizations into other locales. This is mostly just string translation. You can get volunteers or use a service like [PhraseApp](https://phraseapp.com/) to translate your app (who conveniently have [a RubyMotion Gem](https://github.com/phrase/motion-phrase)).

When you upload the new binary, Apple automatically detects the new languages your app is available in and denotes that in the left column of your app's App Store page. I18n and l10n are certainly not easy, but the non-English speaking world will thank you for it.
