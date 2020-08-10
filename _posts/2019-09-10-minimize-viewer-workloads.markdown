---
layout: post
title: Minimizing Viewer workloads - loading models partially with selected components and features only
date: 2019-09-10 00:00:00 +0300
description: Quite curiously, we’ve often had inquiries about mechanisms to tree shake Viewer’s loading process and load only what’s minimally required to render the model or just a part of it, which is effective when it comes to boosting load times and overall performance under challenging conditions such as those on mobile devices. # Add post description (optional)
img: screenshot.png # Add image post (optional)
tags: [] # add tag
---
JQuite curiously, we’ve often had inquiries about mechanisms to tree shake Viewer’s loading process and load only what’s minimally required to render the model or just a part of it, which is effective when it comes to boosting load times and overall performance under challenging conditions such as those on mobile devices. So let’s talk through some of the options available today.

One of the major difficulties when it comes to loading larger models is with loading the model database due to its huge size (could be several GBs) and limited bandwidth. To overcome this it is possible to suppress the loading of model data with the below options (and naturally relevant features such as the model data and property panels would be no longer available, see live demo [here](https://jsbin.com/fozamus/2/edit?js)):

```
const options = { skipPropertyDb: true}
viewer.start/loadModel/loadDocumentNode(document, geometry,options)
```

Yet we can still access the model data by persisting it to our own data source through our own custom workflows, see this article [here](https://forge.autodesk.com/blog/working-viewer-data-serverlessly-thumbnails-screesnshots-and-serverless-upload) for more details.

And to cut down the rendering workloads we can selectively load only certain components/nodes of the model by passing in their dbids as an array to the load options (see live demo [here](https://jsbin.com/wuquhus/10/edit?js)):

```
const options = { ids:[1,2,3 ...] }
viewer.start/loadModel/loadDocumentNode(document,geometry,options)
```

It's also possible to suppress certain built-in features (measurement, sectioning etc.) that are loaded by default - simply try these with your initialization options below:

```
const options = {disabledExtensions: {
measure:true,
viewcube:true,
layermanage:true,
explode:true,
section:true
hyperlink:true,
bimwalk:true,
fusionOrbit:true
//...
}}
const viewer = new Autodesk.Viewing.GuiViewer3D(container, options)
```

Furthermore we can switch to a GUIless version of Viewer without all the built-in GUI components including all the toolbars, panels etc. All functional APIs would remain available otherwise (but you'd still need to suppress built-in extensions manually):

```
const viewer = new Autodesk.Viewing.Viewer3D(container, options)
```

Now let’s put these through their paces and see the memory economy - this moderately large model would take several minutes to load and will take up around 100MB in memory (on Chrome v80 and Viewer v7.13):

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/DBD9904F-97E9-444A-A804-D770757AF00F.jpeg)

And we disabled the property database and chose to load just a few segments, everything turns up in a few seconds and memory dropped to under 20MB:

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/60A2CFD0-AAD7-43BF-8AB2-EDE9663675B0.jpeg)

Finally as a side note, with SVG elements (markups etc) we can to boost rendering performance using "transform" to set/move their positions as those translations would be GPU accelerated - see more [here](https://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/).
