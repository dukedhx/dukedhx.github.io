---
layout: post
title: Bootstrap your Forge real-time IoT apps using full-stack designs with React Hooks and FeathersJS
date: 2019-04-06 13:32:20 +0300
description: Over the years we've had queries constantly about how to integrate Viewer with real-time data to create data intensive IoT apps so today let's walk through a beginner friendly sample to get us started... # Add post description (optional)
img: bcf26ec9-89ee-4826-b915-66c5197df2a1.gif
---

## Objectives

- Add Markups to Viewer to visualize sensor readings
- Integrate React Hooks components with Viewer and its extensions
- Implement full-stack Node backend for real-time data communication

## Sample Code

https://github.com/dukedhx/viewer-iot-react-feathersjs

## Live Demo

<iframe allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts" src="https://codesandbox.io/embed/objective-architecture-1wvd7?fontsize=14&amp;hidenavigation=1&amp;theme=dark" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" title="objective-architecture-1wvd7"></iframe>

## Appending Markups

Our sample here appends the markups by creating DOM elements to visualize sensor data and placing them ahead of the Viewer's scene by their coordinates converted from the world positions of model nodes corresponding to the senors' positions while maintaining their positions as we navigate through the model scene - the very same approach brought to us by Augusto and Xiaodong previously and you may find their articles below:

- https://forge.autodesk.com/blog/placing-custom-markup-dbid
- https://forge.autodesk.com/blog/create-pushpin-markup-svg

And refer to these if you are after appending 3D geometry as markers to the scene:

- https://forge.autodesk.com/blog/3d-markup-icons-and-info-card
- https://forge.autodesk.com/blog/custom-models-forge-viewer

## Integrating React Components with Viewer

Since the markups are DOM elements rendered from React components we'd need to come up with a mechanism for Viewer, which has to be detached as an external library not possible to import into a React component, to . Hence we'd need to call ReactDOM within the scopes of Viewer's callbacks and extension to render React components:

```
// src/client/iconExtension.js

const container = document.createElement('div')
    this.viewer.clientContainer.append(container)

    ReactDOM.render(
      <IconContext.Provider value={this.iconContextObject}>
        {this.icons.map((icon, i) => (
          <Icon
            key={i}
            id={icon.extId}
            labelClassName="markup update"
            spanClassName="temperatureBorder"
            pattern="%1&#176;C"
          />
        ))}
      </IconContext.Provider>,
      container
    )
    setTimeout(() => this.updateIcons(), 500)
```

## Connecting Data

Introducing FeathersJS - a full-stack Node.js framework for building real-time apps. With it you'd have all the wherewithal at your disposal out of the box to handle real-time data both in the UI and the  backend using a symmetric set of APIs. Under the hood FeathersJS should actually be chalked up as a meta-framework that upon "usual suspects" such as Express and Socket.IO, except that FeathersJS wraps its APIs around these building blocks and saves us the groundwork and chores so we could concentrate on the apps themselves.

For our sample we simulate sensor feeds with random mock data and use externalIds (which are basically the same as the identifiers (component IDs) from the model editors) to associate sensors with component nodes in Viewer:

```
// src/server/index,js

app.configure(socketio());
// Register an in-memory messages service
app.use('/messages', new MessageService());

//...

setInterval(() => app.service('messages').create(readings.reduce((o,e)=>Object.assign(o,{[e]:Math.floor(Math.random()*100)}),{})), 2000); // Mock data generator - replace with your own real sensor data feed (try any mqtt node client etc.)
```

FeathersJS is actually an alternative to the more mainstream choice of the same kind known as the MeteorJS, of which we can find a sample with Viewer below: https://github.com/dukedhx/viewer-meteor-sample

## Putting it All Together

As usual let's stick with [react-scripts](https://www.npmjs.com/package/react-scripts) to scaffold the project except that for this time around we cranked it up a notch with [react-app-wired](https://github.com/timarney/react-app-rewired) along with custom build scripts to customize the build process without ejecting, which is also quite a common concern raised by developers that we often had to address ... :

```
// bootstrap-build.js

const checkRequiredFilesPath = 'react-dev-utils/checkRequiredFiles';
require(checkRequiredFilesPath);
require.cache[require.resolve(checkRequiredFilesPath)].exports = () => true;
// run original script
if(process.argv[2]=='build'){
const fs = require('fs');
!fs.existsSync('./public') && fs.mkdirSync('./public');
require('react-app-rewired/scripts/build');
}
else require('react-app-rewired/scripts/start');
```

## What Next?

Try the below challenges yourself and let us know how you go:

\- Connect real world sensor feeds using protocols like MQTT/AMQP, see an example in point [here](https://github.com/cyrillef/Pier9.IoT)
\- Enable two-way real-time communication to accept process user input
\- Deploy to cloud service leveraging AWS IoT and/or Azure IoT hub
