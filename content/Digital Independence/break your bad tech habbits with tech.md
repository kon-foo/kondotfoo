---
description: Use a browser extension to break with your bad habbits and redirect yourself to a good alternative every time you find yourself on a Big Tech website.
draft: false
tags:
  - UnpluggTrump
  - "#DIDit"
  - DigitalSovereignty
  - digitallyindependent
title: break your bad tech habbits with tech
date: 2026-02-01
---
>[!info]  This article is part of my ongoing effort to promote #digitallyindependent

An easy first step on the way to independence from Big Tech can be to find alternatives for the small things. The small, everyday services that you rely on and that you might use without even noticing. For example when I am in lack of an English term,  I hit Ctrl+T to open a new browser tab, type "tra", hit enter and end up on "translate.google.com" before I can even think about what I am doing.  Instead of giving Google the next clue about my persona that it can turn into marketing data, I might as well use [DeepL](https://deepl.com). DeepL is great! Some would say it is superior to Google Translate. I have known that for a long time and I did use DeepL in the past. However the automatism that my brain is wired to still brings me back to Google again and again.
The problem is - as it is so often - to break with habbits and form new ones. So I thought to my self: Why not use technology for a psychological hindrance. What could possibly go wrong?

## The solution: Redirection
If I can’t control myself, maybe my tech can. The solution is redirections. Whenever I open a website I want to avoid, I want my browser to say 'Nah-ah! You’re going somewhere else, my friend,' and redirect me to the alternative.
There is a myriad of ways to achieve this, ranging from simple to complex from local to network wide. And while it would be fun to implement this for my entire household to see the reactions, I decided to start with an easy, local solution: A browser extension. 

### Let's tamper
While there are extensions for exactly this purpose, these are a little to [opinionated](https://addons.mozilla.org/de/firefox/addon/privacy-redirect/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search), [over-engineered](https://addons.mozilla.org/de/firefox/addon/libredirect/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search), or just [not maintained](https://addons.mozilla.org/de/firefox/addon/redirector/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search) anymore. I was about to do it myself and throw another unmaintained Firefox extension into the ring, when I remembered that there are extensions that let you add arbitrary JavaScript Code to any website. After some research I settled on [Violentmonkey](https://violentmonkey.github.io/get-it/). It is [open source](https://github.com/violentmonkey) in contrast to its bigger brother Tampermonkey. Furthermore it is well-aged, actively maintained, available on all major browsers and has a big user base. Of course this kind of extensions requires broad permissions and could potentially harm you really badly if taken over by an malicious actor, but I decided to take the risk an use it for a couple of weeks/month to break with my habbits.

Bonus Points: It super easy and can much more than just redirect. 

#### HowTo
1. Install the Violetmonkey Browser Extension ([Firefox](https://addons.mozilla.org/firefox/addon/violentmonkey/), [Chrome](https://chrome.google.com/webstore/detail/violent-monkey/jinjaccalgkegednnccohejagnlnfdag))
2. Browse to the website you plan to avoid, e.g. [translate.google.com](https://translate.google.com/) 
3. Open the Violentmonkey menu (In case of Firefox, click on the little puzzle symbol in the top right, then click on Violenmonkey)
4. Click on the ➕ button.
5. A script editor opens, below the existing comments block (indicated by `//`) enter the following line:
```javascript
...
// ==/UserScript==
window.location.replace("https://www.deepl.com/translator");
```
6. Save.
7. Success! Open [translate.google.com](https://translate.google.com/) in a new tab and within the blink of an eye, you should be redirected to DeepL or whatever you configured.

#### Getting Fancy
Now the above is the bare minimum. It does what we've set sail for.  But we don't have to stop here. With script injection, the possibilities are limitless. Maybe we do not want to get redirected when we click on a link, but **only when we enter the URL directly**. This can be achieved by checking the `document.referrer`, a JavaScript property that holds the URI of the page that linked to the current page.  This can be achieved with this simple change:
```javascript
// Only redirect if there is no referrer (direct entry or bookmark)
if (document.referrer === "") {
    window.location.replace("https://www.deepl.com/translator");
}
```

I soon realized that I have another use-case: Hard to admit, but sometimes I do not want to get redirected. For example [openstreetmap.org](/#map=11/55.6154/12.5632) is amazing, but it is not a complete alternative to Google Maps. Sometimes I just want to use Google Maps. To get up-to-date information on some location, or use Street View to check something out. With the above I could search for Google Maps in [DuckDuckGo](https://duckduckgo.com/) and click on a link, to have `document.referrer` populated and not get redirected... but that's quiet a nuisance. How about, instead have a info bar at the top and get a fair chance to abort the redirect within a certain time? Well, here you go:

![[auto-redirect-violentmonkey-script-with-cancellation-preview.png]]

The source code got quite a bit long. You can find it in [this](https://codeberg.org/kon-foo/violentmonkey-userscripts/src/branch/main/auto-redirect-empty-referrer.js) repository.

### A word on SPAs
If you studied the source code for the more complex example carefully, you might have noticed that the match patterns that Violentmonkey needs to decide when to inject our script, look a bit different. In fact, Violentmonkey even gives you a warning when you use the script: `Bad pattern: missing "/" for path in *://maps.google.com*` This bad pattern is necessary because Google Maps is a Single-Page-Application or SPA. This basically means that the app is dynamically changing the content of the page and of the address bar, without really reloading the page. So in the eyes of Violentmonkey we stay on the root URL google.com/maps the entire time, even though Maps immidiately updates the address bar to something with a trailing `/`. You might encounter similar problems when attempting to use Violentmonkey on other SPAs, so keep this in mind.

## Alternatives
Here are some other (potential) ways of achieving the same. "Potential" because I haven't thought them through, let alone tested them. But if you need another solution, this might be a starting point 
- Redirect Extensions, e.g. for [Firefox](https://addons.mozilla.org/de/firefox/search/?q=redirect) or [Chrome](https://chromewebstore.google.com/search/Redirect)
- `/etc/hosts` 
- Pi-holle DNS
- Proxy in my Tailnet