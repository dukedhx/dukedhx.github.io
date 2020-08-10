---
layout: post
title: Meet the Forge Tools hub - JAMStack/Serverless Tools to download Derivatives (SVF) and View Models Offline (PWA)
date: 2020-03-06 13:32:20 +0300
description: Personally I’ve always felt that we have yet to harness the full power of modern browsers, especially with all the potential made possible by the advent of H5 and modern APIs. With this mind I"ve come up with this set of browser side tools to address common concerns like saving the derivatives (especially SVF), downloading and uploading objects in chunks and caching the models for offline view. # Add post description (optional)
img: ezgif-7-126ea6987c7a.gif # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: []
---
Personally I’ve always felt that we have yet to harness the full power of modern browsers, especially with all the potential made possible by the advent of H5 and modern APIs. With this mind I"ve come up with this set of browser side tools to address common concerns like saving the derivatives (especially SVF), downloading and uploading objects in chunks and caching the models for offline view.

# The Whereabouts

- Production: https://forge-tools-hub.now.sh/
- Playground: https://codesandbox.io/s/github/dukedhx/forge-tools-hub/tree/master
- Code: https://github.com/dukedhx/forge-tools-hub/

# The Gospels

All tools are 100% JAMStack/Serverless, which means they can be deployed as static web pages w/o a service backend (while still being able to leverage essential Serverless capabilities - we will delve more into this topic in my upcoming articles soon, as I’ve promised a few times already ;p)! This approach does not only save us the precious compute/traffic resources, but also provides extra security for confidential data in transmission such as your app credentials and access tokens - none of your data would go past our backend as the browser client communicates directly with the target services so you can even safely use the tools (the Requester) to request access tokens. Having said this it’s still recommended to generate access tokens with your own client/tool of choice for security reasons.

The Tools

Now let’s quickly go through what’s included in this, namely, “curated collection of Forge tools with boilerplate code to build SSR Forge apps”:

- SVF Saver: Download SVF files for an extracted model from Forge with Service Worker capturing the derivatives as Viewer renders the model- Downloader: Download a derivative or any web resource in chunks (so the request won’t get timed out) and in parallel for better efficiency- HTTP Requester: Effortlessly make any (browser supported) HTTP requests to execute simple tasks like access tokens, model translation etc - Derivative Downloader: Parse a derivative manifest for an extracted model on Forge and download selected derivatives (SVF or other supported formats)
- PWA: Cache and view models offline - PWA ready
- Uploader: Upload an object in paralleled chunks - requires the recipient service to explicitly expose forbidden headers if needed
- Viewer Options Generator: Generate all possible Viewer init/load options such as transform metrics easily …
- Tutorial/Playground: Curated collection of interactive Forge tutorials & playgrounds

# Demo

## Requesting Forge Access Tokens and Translation Jobs etc.

You can use the Request tool to easily fetch access tokens, call translation jobs and since the app has no backend and runs completely in your browser it's almost equivalent to using other popular tools (Postman, cURL etc) for direct interaction with the Forge APIs - note a new, separate window tab will open to receive access tokens due to security limitations, see screencast [here](https://www.screencast.com/t/bORg4D5G) for instructions:

<iframe allowfullscreen="" class="embeddedObject shadow resizable" frameborder="0" height="500" mozallowfullscreen="" name="embedded_content" scrolling="no" src="https://www.screencast.com/users/dukedhx/folders/Default/media/609ca653-c4bc-4368-9f10-0d6932df2989/embed" style="overflow:hidden;" type="text/html" webkitallowfullscreen="" width="800" id="embedded_content"></iframe>

## Saving Derivatives (SVF etc.)

You can either download the derivatives directly with the Derivative Downloader tools - see screencast [here](https://www.screencast.com/t/ZUkIn2yQOX):

<iframe allowfullscreen="" class="embeddedObject shadow resizable" frameborder="0" height="500" mozallowfullscreen="" name="embedded_content" scrolling="no" src="https://www.screencast.com/users/dukedhx/folders/Default/media/76ce407b-0a25-4f1f-b4e1-6fdba3552895/embed" style="overflow:hidden;" type="text/html" webkitallowfullscreen="" width="800" id="embedded_content"></iframe>

Or save the SVFs as they load on Viewer - see screencast [here](https://www.screencast.com/t/Ds2vZ1hh) (requires Service Worker support):

<iframe allowfullscreen="" class="embeddedObject shadow resizable" frameborder="0" height="500" mozallowfullscreen="" name="embedded_content" scrolling="no" src="https://www.screencast.com/users/dukedhx/folders/Default/media/e66bb62b-1a8f-4c4d-8a7f-dad8cd82b3d7/embed" style="overflow:hidden;" type="text/html" webkitallowfullscreen="" width="800" id="embedded_content"></iframe>

P.S.: Did you know you can also download the derivatives with the magic of [Node.js Forge CLI tools](https://www.npmjs.com/package/forge-cli-utils) by Petr? At the time of writing my derivative tweaks are yet to be published so feel free to hit the Github repo of the tools [here](https://github.com/petrbroz/forge-cli-utils) directly to take advantage of it - See screencast here:

<iframe allowfullscreen="" class="embeddedObject shadow resizable" frameborder="0" height="500" mozallowfullscreen="" name="embedded_content" scrolling="no" src="https://www.screencast.com/users/dukedhx/folders/Default/media/598475d5-278e-46c6-a765-a8bda62b950d/embed" style="overflow:hidden;" type="text/html" webkitallowfullscreen="" width="800" id="embedded_content"></iframe>

## Viewing Models Offline & PWA

Head up to the PWA tool to cache and load models for offline view (requires Service Worker support) - see screencast [here](https://www.screencast.com/t/7PtEevQgou):

<iframe allowfullscreen="" class="embeddedObject shadow resizable" frameborder="0" height="500" mozallowfullscreen="" name="embedded_content" scrolling="no" src="https://www.screencast.com/users/dukedhx/folders/Default/media/0f4431bd-a9c2-42a8-b7ff-241cad687e81/embed" style="overflow:hidden;" type="text/html" webkitallowfullscreen="" width="800" id="embedded_content"></iframe>

And simply add the Forge Tools site to your home screen on a compatible device (for iOS, iPadOS, Android etc, requires Service Worker support) - see screencast [here](https://www.screencast.com/t/KkxfyKFCAim):

<iframe allowfullscreen="" class="embeddedObject shadow resizable" frameborder="0" height="500" mozallowfullscreen="" name="embedded_content" scrolling="no" src="https://www.screencast.com/users/dukedhx/folders/Default/media/0f4431bd-a9c2-42a8-b7ff-241cad687e81/embed" style="overflow:hidden;" type="text/html" webkitallowfullscreen="" width="800" id="embedded_content"></iframe>

## Downloading Large Objects in Chunks

For larger objects we might get timed out if our request takes too long so use the Downloader tool to fetch them in chunks - see screencast [here](https://www.screencast.com/t/Ds2vZ1hh):

<iframe allowfullscreen="" class="embeddedObject shadow resizable" frameborder="0" height="500" mozallowfullscreen="" name="embedded_content" scrolling="no" src="https://www.screencast.com/users/dukedhx/folders/Default/media/a7c6e900-9ad5-4c0d-98f6-1a458a99cf7d/embed" style="overflow:hidden;" type="text/html" webkitallowfullscreen="" width="800" id="embedded_content"></iframe>

P.S.: Currently on certain browsers you might need to manually input the content length of the object (by making a [HEAD request](https://forge.autodesk.com/en/docs/model-derivative/v2/reference/http/urn-manifest-derivativeurn-GET/) to get its length) and this should get resolved soon once I work out an issue in our backend with the Engineering team. And did you know you can also download in chunks with the magic of [Node.js Forge CLI tools](https://www.npmjs.com/package/forge-cli-utils) by Petr?

## Uploading Large Objects in Chunks

Similarly you can upload large objects in chunks with the Uploader tool - see screencast [here](https://www.screencast.com/t/Ds2vZ1hh):

<iframe allowfullscreen="" class="embeddedObject shadow resizable" frameborder="0" height="500" mozallowfullscreen="" name="embedded_content" scrolling="no" src="https://www.screencast.com/users/dukedhx/folders/Default/media/d459539d-2aab-44d6-91c8-3abaaa3e29e1/embed" style="overflow:hidden;" type="text/html" webkitallowfullscreen="" width="800" id="embedded_content"></iframe>

P.S.: Did you know you can also upload in chunks with the magic of [Node.js Forge CLI tools](https://www.npmjs.com/package/forge-cli-utils) by Petr?

## And So Much More!

You can easily use the Requester, Uploader and Downloader tools for other web/cloud services like AWS, Azure, GCD or even your own since the request headers and body can be customized at will - see [here](https://hackmd.io/@RXhmeebQS7OsPaoAEdnP5Q/SkbznYWQ8) for detailed instructions …

Limitations

- Currently the Downloader tools are impacted by the lack of access to the “Content-Length” header and some browser clients may not be able to obtain the object length properly - I am working with our Engineering to resolve this and I will come back with updates soon.
- Degraded UX experience is to be expected with small form devices like phones - will address this when my schedule decompress hopefully soon ;p.
- Compatibility of PWA and SVF Saver tools is limited to modern browsers with full support for Service Worker, Cache and LocalStorage APIs.
- All functionality has only been tested on Chrome v70 (Win) and v80 (Mac).
