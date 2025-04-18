```markdown
# TheFlixerTV: Block Popups Only

> 🛡️ A tiny userscript to remove only TheFlixerTV’s toast pop‑ups and fixed‑position ad iframes—leaves your video untouched.

![screenshot-before](https://user-images.githubusercontent.com/llamaboi55/placeholder-before.png)  
*Before*  
![screenshot-after](https://user-images.githubusercontent.com/llamaboi55/placeholder-after.png)  
*After*

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
   ```

4. Visit **theflixertv.to** and enjoy an ad‑free experience!

---

## Edge Extension

If you prefer a standalone Edge add‑on, see the [`edge/`](./edge/) folder for a Manifest V3 extension you can load unpacked in `edge://extensions`.

---

## Support & Donations

If this script saved you from those pesky pop‑ups, feel free to support its development:

[![Sponsor me on GitHub](https://img.shields.io/badge/Sponsor-GitHub-181717?style=flat&logo=github)](https://github.com/sponsors/llamaboi55)  
[![Donate via PayPal](https://img.shields.io/badge/PayPal-Donate-003087?style=flat&logo=paypal)](https://paypal.me/llamaboi55?country.x=US&locale.x=en_US)

Every tip—big or small—helps keep this project alive. Thank you! 🙏

---

## Contributing

1. Fork the repo  
2. Create your feature branch (`git checkout -b feature/my-change`)  
3. Commit your changes (`git commit -am 'Add awesome feature'`)  
4. Push to the branch (`git push origin feature/my-change`)  
5. Open a Pull Request

---

Happy streaming,  
— _Gili (llamaboi55)_  
```
