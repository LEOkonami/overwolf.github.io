---
id: ads-sdk-overview
title: Overwolf Advertising SDK
sidebar_label: Ads SDK overview
---

The Overwolf advertising SDK is a JavaScript library that allows developers to integrate ads into their Overwolf applications. Ads are managed, filtered and hosted by Overwolf. The Advertising SDK is how you as a developer can show ads in your apps and monetize your traffic, but it's important to do it right - please contact us and we'll help integrate forgivable ads that do not hurt user experience.

:::important
Ads will be served to your app only after Overwolf enables them for you, contact us at developers@overwolf.com when your app is ready to show ads.
See [here](how-to-test-your-app#ads) how to test ad implementation with a placeholder ad before app submission.
:::

#### Getting help and reporting bugs

If you have encountered problems with the advertising SDK, please let us know by contacting developers@overwolf.com, or just message us directly over the [Developers Slack](http://overwolfdevs.slack.com).

:::tip Sample app
The ads-sample demonstrates the usage in the ads API, how to display an ad in an in-game window and desktop window, when to hide it, when to restore it, etc.
In our [APIs sample apps repository](https://github.com/overwolf/apis-sample-apps), you can find and download a  sample app.
:::

The following scenarios and principles are demonstrated in the Ads sample app:

* Position and Dimensions:  
  * Ads box is implemented in the top-level component with absolute positioning.
  * Ad container is <= container div, so the ad won't cut.
  * Ad box is fixed and without scroll bar.
  * Ads box is not refreshed when navigating components inside the same window. 
  * The ad placement area is not scrollable.
  * Tabs: Ads box is not refreshed when switching tabs, and its position is permanently kept.
  * No menus or pop-up windows on top of the ad.
  * Ad placement dimensions are being kept while resizing the app window. 
* Clickable:
  * Ads are clickable, and clicking on an ad will open the pre-defined browser (OW or user browser, as set in the manifest).
* Focus:
  * There is no ad refresh when losing focus (clicking on a different spot in the app, the game, or outside the app).
  * When the game gain focus, the ad is not loaded if the app window was previously minimized and not restored.
* DPI: 
  * The app proportions, and ads dimensions are not cut on DPI 125% / 150% and resolutions.
* Minimize / Restore / Closing:
  * Ad is removed when the user presses Win Key + D and refreshes on restore.
  * Ad is removed when the user minimized the app through the app minimize "_" button and refreshed on restore.
  * Ad is removed when the user minimized the app through the in-game hotkeys (if defined) and refreshed on restore.  Note that on the current version of this sample app, no hotkeys are implemented yet.
  * Ad is removed, then the user closes the app.

It's a great place to get started - All the samples in this repository are built with JS code and demonstrate primary usage in the API.

## Getting Started

The most simple and basic usage of this library can be done by adding a few lines of code into your app window’s HTML file:

```html
<div id="ad-div"></div>
<script src="https://content.overwolf.com/libs/ads/latest/owads.min.js" async onload="onOwAdReady()"></script>
<script>
  // This isn't a recommended way of holding the ads object - we only want to demonstrate
  // some concepts for the sake of documentation.
  let _owAd = null;
 
  // Setup a fallback timeout to check that onOwAdReady was called.
  // The owads.min.js might have been blocked.
  setTimeout(() => {
    if (!owAd) {
      console.error("Couldn't load owads.min.js!");
    }
  }, 20000);
 
 
  function onOwAdReady() {
    if (!OwAd) {
      // This scenario might happen if the user is running behind an ISP/public 
      // router that might have detected the library as an ad and redirected the
      // request to an error page.
      // The app should decide how to handle this scenario (trigger an event, 
      // fallback to a place holder etc...)
      return;
    }
 
    // The creation of an OwAd object will automatically load an ad (so no need to
    // call refreshAd here).
    _owAd = new OwAd(document.getElementById("ad-div"));
 
    _owAd.addEventListener('ow_internal_rendered', () => {
      // It is now safe to call any API you want ( e.g. _owAd.refreshAd() or 
      // _owAd.removeAd() )
    });
  }
</script>
```

#### This code snippet does the following

1. Adds a div to your page with the id `ad-div`
2. Loads the Overwolf ads SDK library from Overwolf’s CDN
3. When the script is loaded, creates a new OwAd instance that will tell the script to fetch an ad. The HTML element passed on to this constructor will then be used to display the ad itself.

#### Notes regarding the snippet above

* As you can see, the script tag is added with an `async` attribute. This means that the script is loaded asynchronously and will not interfere with your app functions nor slow down it’s loading time. The downside for this is that the script may take time to load and be ready, and may not be immediately available. To overcome this, we are using the `onload` attribute and providing it with the name of a function to be called when the script has finished loading. In the above example it is called `onOwAdReady`.

*   When calling a new `OwAd()` we provide it with a DOM element. In this example we are getting the instance of the element by calling `document.getElementById`. However, you may use any other way to get the DOM element. You may use `document.querySelector`,  jQuery or any other framework you wish. It will work as long as the provided element is an HTML element available at the DOM. If the element does not have an ID, it will automatically be assigned one.

## Changes to your app’s manifest.json

### Set permissions

The Overwolf Advertising library uses Overwolf APIs to improve ad targeting for users. Therefore you should add the following permissions to your app’s manifest.json file:

```json
"permissions": ["Extensions", "Streaming", "Profile", "GameInfo"]
 ```
### Enable Google’s syndication

In order to get impression tracking to work, you will also need to enable a work-around that allows requests to Google’s syndication servers. This is done by setting `"protocol_override_domains" : {"googlesyndication" : "http"}`, under the manifest.json "data" property:
 
```json
 "data": {    
    "protocol_override_domains" : {"googlesyndication" : "http"}
    }
 ```

### Set manifest flags

The following App Window Data flags should be set to `true`:

* [block_top_window_navigation](../api/manifest-json#windows-block_top_window_navigation) -  Stop non `_blank <a>` ads from "taking-over" the entire app’s window.
* [popup_blocker](../api/manifest-json#popup_blocker) – Prevents new browser windows from being opened automatically using scripts.
* [mute](../api/manifest-json#windows-mute) - Mute sounds in window.

## Guidelines for ad integration

Unfortunately the advertising industry is still mostly designed to work with normal web pages, and not single page apps like your Overwolf app. This means that we are often limited in how we can interact or display ads. 

For example, app behavior is at risk of flagging fraud protection solutions our advertising partners use. To prevent this from happening make sure:

1) Your ad container is ALWAYS visible in the app screen. Invisible or hidden ad containers increase your risk. 
2) Once a new OwAd object is configured, do not modify it's properties including dimensions, visibility, opacity, display style, z-index or anything else. Changing containers can cause ads to stop displaying properly or even being flagged for fraud. 

If you encounter any issues or need advice in setting up effective, compliant ad experiences - talk to us!
