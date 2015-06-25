---
layout: default
title: Browser &amp; device support
---

<span class="intro-graf">
The Famous Engine works in most modern browsers. It's also compatible with tools like Cordova that allow you to package your project as a native application.
</span>

## Browsers

**Chrome, Safari, and Firefox have first-class support.** Famous should, however, work in most modern browsers with HTML5/CSS3/JavaScript support.

**Internet Explorer:** If you are working with Microsoft browsers, it should be noted that **all versions of IE have a bug** with respect to the CSS3 `preserve-3d` attribute, which specifically affects 3D interfaces. We are currently working with Microsoft to resolve this bug.

Famous is actively tested in the following browsers:

* Chrome
* Firefox
* Safari
* IE11+

## Devices

Famous supports modern desktop computers, laptops, and mobile devices. Famous is actively tested on the following devices:

* **Android phones:** Nexus 4, Nexus 5, Nexus 6, Moto G
* **Android tablets:** Nexus 7, Nexus 9
* **iOS phones:** iPhone 4S, iPhone 5/5C, iPhone 5S, iPhone 6
* **iOS tablets:** iPad 3/4, iPad Air, iPad Mini, iPad Mini Retina

**Television sets:** We have performed several experiments running Famous on TV sets, including Samsung and LG sets, but these devices are not officially supported.

**Other devices:** IOT and VR devices are not officially supported at this time, although we have performed experiments with them successfully.

## Known browser- & device-specific issues

Each browser and device comes with its own set of quirks. Understanding their limitations is important when developing applications that are cross-browser and device compatible. Below are some of the known issues you may come across while developing with Famous.

### All browsers

**Performance degradation when using semi-transparent windows:** Frame-rate in the browser will take a serious hit when semi-transparent windows lie over them. Be careful when organizing the windows on your desktop, as this can lead to very confusing performance drops.

### iOS7 iPad

**Miscalculated window.innerHeight:** iPads that are running iOS7 miscalculate `window.innerHeight` when they are in landscape orientation. This poses issues for developers relying on the window's size for laying-out components.

### Android

**Bitmap scaling artifacts:** Images that are scaled smaller before being displayed will have their bitmap affected. If that scale transform is then removed, the image will not be displayed using its actual bitmap, due to Android's eager optimization to cache the initial bitmap.

[0]: index.html
[1]: cordova-phonegap.html
[2]: http://Famous
[3]: mailto:support@Famous
[4]: licensing.html
