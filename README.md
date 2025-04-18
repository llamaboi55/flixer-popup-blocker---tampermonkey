# flixer-popup-blocker---tampermonkey
# TheFlixerTV: Block Popups Only

> 🛡️ A tiny userscript to remove only TheFlixerTV’s toast pop‑ups and fixed‑position ad iframes—leaves your video untouched.
\

---

## Features

- **Toast‑ad cleanup**  
  Removes any `<div>` using the site’s `animation: … running show` keyframes.
- **Ad‑bar removal**  
  Strips out fixed‑position `<iframe>` elements (the bottom ad bars), **but** keeps the main player iframe (`#iframe-embed`) intact.
- **Zero configuration**  
  Works out of the box—no options, no popups, no breakage.

---

## Installation

### Tampermonkey / Greasemonkey

1. Install [Tampermonkey](https://www.tampermonkey.net/) or [Greasemonkey](https://www.greasespot.net/).  
2. Click the extension’s icon → **Create a new script**.  
3. Replace all boilerplate with the code below and **Save**:

   ```js
   // ==UserScript==
   // @name         TheFlixerTV: Block Popups Only
   // @namespace    gili.blocker
   // @version      1.6
   // @description  Remove only the site’s toast ads and fixed‑position ad iframes—leaves your video alone
   // @match        *://theflixertv.to/*
   // @match        *://*.theflixertv.to/*
   // @run-at       document-start
   // @grant        none
   // ==/UserScript==

   (function() {
     'use strict';

     function removeAds() {
       // 1) Remove any DIV using their "show" keyframes animation:
       document.querySelectorAll('div[style]').forEach(div => {
         const s = div.getAttribute('style');
         if (s.includes('animation') && s.includes('running show')) {
           div.remove();
         }
       });

       // 2) Remove only fixed-position IFRAMES (the ad bars), but skip the real player:
       document.querySelectorAll('iframe').forEach(iframe => {
         const cs = window.getComputedStyle(iframe);
         if (iframe.id !== 'iframe-embed' && cs.position === 'fixed') {
           iframe.remove();
         }
       });
     }

     // Run on startup
     removeAds();

     // Watch for any injections
     new MutationObserver(removeAds)
       .observe(document.documentElement, { childList: true, subtree: true });
   })();

then just refresh and... done (:
