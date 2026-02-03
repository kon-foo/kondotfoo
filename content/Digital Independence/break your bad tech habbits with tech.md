---
description: Use a browser extension to break with your bad habbits and redirect yourself to a good alternative every time you find yourself on a Big Tech website.
draft: true
tags:
  - UnpluggTrump
  - "#DIDit"
  - DigitalSovereignty
  - digitallyindependent
title: break your bad tech habbits with tech
date: 2026-02-01
---
>[!info]  This article is part of my ongoing effort to get #digitallyindependent

An easy first step on the way to independence from Big Tech can be to find alternatives for the small things. Use [DeepL]() instead of Google Translate, DuckDuckGo instead of Google Search. Doing it once is easy. The problem is - as it is so often - to break with existing habbits and form a new one. Let's take translations as an example. DeepL is great! And some would say it is superior to Google Translate. I have known that for a long time and I used DeepL in the past. However something in my brain is deeply wired towards Google Translate and whenever I need a quick translation I hit Ctrl+T to open a new Browser tab, type "tra" and it enter and end up on "translate.google.com" before I can even think about what I am doing.
So I thought to my self: Why not use technology for a psychological hindrance. What could possibly go wrong?

## The solution: Redirection
If I can’t control myself, maybe my tech can. The solution is redirections. Whenever I open a website I want to avoid, I want my browser to say, 'Nah-ah! You’re going somewhere else, my friend,' and redirect me to the alternative.
There is a myriad of ways to achieve this ranging from simple to complex from local to network wide. And while it would be fun to implement this for my entire household to see the reactions, I decided to start with an easy, local solution: A browser extension. 

### Let's tamper
While there are extensions for exactly this purpose, these are a little to [opinionated](https://addons.mozilla.org/de/firefox/addon/privacy-redirect/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search), [over-engineered](https://addons.mozilla.org/de/firefox/addon/libredirect/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search), or just [not maintained](https://addons.mozilla.org/de/firefox/addon/redirector/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search) anymore. I was about to do it myself and throw another unmaintained Firefox extension into the ring, when I remembered that there are extensions that let you add arbitrary JavaScript Code to any website. After some research I settled on [Violentmonkey](https://violentmonkey.github.io/get-it/). It is [open source](https://github.com/violentmonkey) in contrast to its bigger brother Tampermonkey, Furthermore it is well-aged, actively maintained, available on all major browsers and has a big user base. Of course this kind of extensions requires broad permissions and could potentially harm you really badly if taken over by an malicious actor, but I decided to take the risk an use it for a couple of weeks/month to break with my patterns

Bonus Points: And it super easy. 

#### HowTo
1. Install the Violetmonkey Browser Extension ([Firefox](https://addons.mozilla.org/firefox/addon/violentmonkey/), [Chrome](https://chrome.google.com/webstore/detail/violent-monkey/jinjaccalgkegednnccohejagnlnfdag))
2. Browse to the website you plan to avoid, e.g. [translate.google.com](https://translate.google.com/) 
3. Open the Violentmonkey menu (In case of Firefox, click on the little puzzle symbol in the top right, then click on Violenmonkey)
4. Click on the ➕ button.
5. A script editor opens, below the comments (indicated by `//`) enter the following line:
```javascript
...
// ==/UserScript==
window.location.replace("https://www.deepl.com/translator");
```
6. Success! Open [translate.google.com](https://translate.google.com/) in a new tab and within the blink of an eye, you should be redirected to DeepL or whatever you configured.

#### Getting Fancy
Now the above is the bare minimum. It does what we've set sail for.  But we don't have to stop here. With script injection, the possibilities are limitless. Maybe we only want to get redirected when we click on a link, but **only when we enter the URL directly**. This can be achieved by checking the `document.referrer`, a JavaScript property that holds the URI of the page that linked to the current page. 
```javascript
// Only redirect if there is no referrer (direct entry or bookmark)
if (document.referrer === "") {
    window.location.replace("https://www.deepl.com/translator");
}
```

I soon realized that I have another use-case: Hard to admit, but sometimes I do not want to get redirected. For example [openstreetmap.org](/#map=11/55.6154/12.5632) is amazng, but it is not complete alternative to Google Maps. Sometimes I just want to use Google Maps. To check up-to-date information on some location, or use Street View to check something out. With the above I could search for Google Maps in [DuckDuckGo](https://duckduckgo.com/) and click on a link, to have `document.referrer` populated and not get redirected... but that feels wrong. How about instead, have a info bar at the top and get a fair chance to abort the redirect within a certain time? Well, here you go:

![[auto-redirect-violentmonkey-script-with-cancellation-preview.png]]

[Here](https://codeberg.org/kon-foo/violentmonkey-userscripts/src/branch/main/auto-redirect-empty-referrer.js) you can find  the complete source code



#### A word on SPAs

## Alternatives
