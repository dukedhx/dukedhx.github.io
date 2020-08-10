---
layout: post
title: All definitely defined! TypeScript definitions updates and React TypeScript sample
date: 2019-01-12 13:32:20 +0300
description: As part of the recent updates to Viewer(v7.9 as of now) and our Node.js client SDK (to 0.7, see release notes [here](https://forge.autodesk.com/blog/nodejs-sdk-update), major updates include missing methods and the Command endpoints) we have now updated our TypeScript definitions as well to reflect those updates. # Add post description (optional)
img: sb233.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: []
---

As part of the recent updates to Viewer(v7.9 as of now) and our Node.js client SDK (to 0.7, see release notes [here](https://forge.autodesk.com/blog/nodejs-sdk-update), major updates include missing methods and the Command endpoints) we have now updated our TypeScript definitions as well to reflect those updates. And for those of you who are new to our definitions please find them [here](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/forge-apis) and [here](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/forge-viewer).

Since we released our Typescript definitions we have been pleasantly surprised by the massive contribution from our developer community - we'd like to express our gratitude here and hats off especially to [@alansmithnbs](https://github.com/alansmithnbs), [@reediculous456](https://github.com/reediculous456) and [@bcmarinacci](https://github.com/bcmarinacci) for their outstanding contributions!

And here's some code samples to get started developing with Forge in TypeScript:

- Essential TypeScript sample for Viewer and Node.js client SDK: https://github.com/Autodesk-Forge/viewer-nodejs-typeview.sample
- Advanced Viewer app sample in Vue with TypeScript and PWA support: https://github.com/Autodesk-Forge/forge-digital-catalog
- Essential React sample with TypeScript and PWA support: https://github.com/dukedhx/viewer-react-typescript-workbox

Among the above I recently released another for React - a sample project intended as a starting point for React, TypeScript and Autodesk Forge Viewer with sought after features like lazy loading, caching and PWA, leveraging [Workbox ](https://developers.google.com/web/tools/workbox/)(a set of libraries and Node modules that make it easy to cache assets and take full advantage of the Service Worker and Cache APIs and their features used to build Progressive Web Apps) as well as key new features of [React 16](https://reactjs.org/blog/2017/09/26/react-v16.0.html) such as the [Hooks](https://reactjs.org/docs/hooks-intro.html) API.

With the Workbox it's extremely easy to cache responses by adding routes to your service worker:

```
//./public/sw.js
workbox.routing.registerRoute(
  /^https:\/\/developer\.api\.autodesk\.com\/modelderivative\/v2\/viewers/,
  new workbox.strategies.CacheFirst({
    cacheName: 'forge-viewer-library',
    plugins: [
      new workbox.cacheableResponse.Plugin({
        statuses: [0, 200],
      }),
    ],
  }),
);
```

So if you'd like to cache models hosted on Forge or your own services simply add routes accordingly:

```
workbox.routing.registerRoute(
 /^https:\/\/developer.api.autodesk.com\/derivativeservice\/v2\/derivatives\/{your model URN goes here}/,
  new workbox.strategies.CacheFirst({
    cacheName: 'forge-viewer-library',
    plugins: [
      new workbox.cacheableResponse.Plugin({
        statuses: [0, 200],
      }),
    ],
  }),
);

workbox.routing.registerRoute(
 /^https:\/\/path\/to\/your\/modelSVF/,
  new workbox.strategies.CacheFirst({
    cacheName: 'forge-viewer-library',
    plugins: [
      new workbox.cacheableResponse.Plugin({
        statuses: [0, 200],
      }),
    ],
  }),
);
```

Note that in our sample we explicitly reference and import the workbox library at the top of our script - this can be managed by your build tools for a more managed/streamlined developer experience and you can use [workbox-webpack-plugin](https://www.npmjs.com/package/workbox-webpack-plugin) for example.

And once the model is cached (loaded once with the network available) it'd load from cache without network from there on:

<iframe allowfullscreen="" class="embeddedObject shadow resizable" frameborder="0" height="912" mozallowfullscreen="" name="embedded_content" scrolling="no" src="https://www.screencast.com/users/dukedhx/folders/Default/media/9bfbaeab-f797-456f-9ced-cfc4b171cbaf/embed" style="overflow:hidden;" type="text/html" webkitallowfullscreen="" width="1024" id="embedded_content"></iframe>

You may also want to brand your app as a PWA by customizing the "manifest.json" which would get picked up by client browser when installing the app:

```
//./public/manifest.json
{
  "short_name": "Your APP",
  "name": "Your APP name",
  "icons": [
    {
      "src": "favicon.ico",
      "sizes": "64x64 32x32 24x24 16x16",
      "type": "image/x-icon"
    },
    {
      "src": "logo192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "logo512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": ".",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff"
}
```

And to read more about this we covered more about PWA previously [here](https://forge.autodesk.com/blog/native-experience-viewer-build-viewers-offline-workflows-progessive-web-apps). You may also find inspirations from the [Digital Catalog sample](https://github.com/Autodesk-Forge/forge-digital-catalog).
