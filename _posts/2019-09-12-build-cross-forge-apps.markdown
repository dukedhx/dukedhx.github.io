---
layout: post
title: Build cross-platform Viewer apps with Reason React Native and NativeScript Angular
date: 2019-09-12 00:00:00 +0300
description: This is an extracurricular (and "final" final) addition to our cross platform series to cover a couple of alternative frameworks (namely Reason React Native in ReasonML and NativeScript in Angular) with a quick note on loading models locally with React Native. # Add post description (optional)
img: untitled.png # Add image post (optional)
tags: [] # add tag
---

Code Samples:

\- Reason React Native: https://github.com/dukedhx/viewer-reason-react-native
\- NativeScript (Angular): https://github.com/dukedhx/viewer-nativescript-angular



## Reason React Native

Reason React Native is a safe & simple way to build React Native apps, using ReasonReact - a safer, simpler way to build React components, in Reason.

But is Reason a new language and why bother to learn it? - it's not! Reason is a new syntax and toolchain powered by the battle-tested language, OCaml. Reason gives OCaml a familiar syntax geared toward JavaScript programmers, and caters to the existing NPM/Yarn workflow folks already know. In that regard, Reason can be considered as a solidly, statically typed, faster and simpler cousin of JavaScript, minus the historical crufts, plus the features of ES2030 you can use today, and with access to both the JS and the OCaml ecosystem!

So by leveraging the ReasonML great type system, expressive language features and smooth interoperability with JavaScript (thanks to BuckleScript), Reason React Native provide bindings for React Native features as components & APIs that are:

- Safe and statically typed with a rock solid type system, but w/o all the hassle of TypeScript (still true of 3.x at least) when developers have often got to wrestle with common pitfalls like type anonymity and null assertions.
- Lighting fast and great interop - Reason can be compiled to JavaScript using the build system, bsb, which finishes incremental builds in less than 100ms with resulting output also tiny.
- Simple and lean, with a focus on simplicity & pragmatism. Reason allows opt-in side-effect, mutation and objects for familiarity & interop, while keeping the rest of the language pure, immutable and functional.
- Great ecosystem & tooling. Use your favorite editor, your favorite NPM package, and any of your favorite existing stack
- Familiar and easy to insert into an existing React Native codebase, exactly like what we are doing with our code sample - adding a Reason component of WebView with Viewer in it to an existing React Native app.

Talk is cheap so let's get to the code! Let's simply take any of your existing React Native project and add Reason React Native as dependencies, which is quite straightforward so see instructions here: https://reason-react-native.github.io/en/docs/install/

Then simply set out our component in JSX:

```
// ./src/app.re
let make = (~url) =>
  <>
        <View style={styles##header}> <Text style={styles##headerText}> "Autodesk Forge"->React.string </Text></View>
        <View style={styles##body}>
        <WebView
          source=WebView.Source.uri(~uri=url, ())
          />
        </View>
  </>;
```

And note how Reason's syntax denotes typing for our style object:

```
// ./src/app.re
let styles =
  Style.(
    StyleSheet.create({
      "body": style(~backgroundColor=colors##white,~flex=11., ()),
      "header": style(~backgroundColor=colors##primary,~flex=1., ()),
      "headerText": style(
          ~color=colors##white,
          ~fontSize=18.,
          ~fontWeight=`_600,
          ~paddingTop=35.->dp,
          ~textAlign=`center,()),
    }),
  );
```

And bear in mind that the official WebView has been split from the main libraries to a separate dependency ("react-native-webview") that needs to be installed separately ... And here we create a reference for it through JS interop with React Native:

```
// ./src/app.re
module WebView ={
  module Source = {
    type t;

    [@bs.obj]
    external uri:
      (
        ~uri: string=?,
        ~method: string=?,
        ~headers: Js.t('a)=?,
        ~body: string=?,
        unit
      ) =>
      t =
      "";
  };
[@react.component][@bs.module "react-native-webview"]
external make: (~source: Source.t=?) => React.element = "default";
}
```

Finally, adding our Reason component to our React app in the JS realm is as simple as referencing its post compilation emit (*.bs.js):

```
// ./App.js
import {make as AppComponent} from './src/App.bs.js';
```

Now we are all set! Time to compile and see it in action on both iOS and Android:

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/Untitled%204_1.PNG)

To set up and run the sample refer to instructions in the README: https://github.com/dukedhx/viewer-reason-react-native

## Loading Models/HTMLs Locally

Previously we covered but unfortunately Service Workers are still not yet fully supported in WebViews on iOS as of now. Then why not fire up a static server to serve the HTMLs and SVFs (remember Viewer only supports HTTP so the traditional file system trick is out of the window (not for Android though and we covered that [here](https://forge.autodesk.com/blog/offline-viewing-android)))? Out of a few choices we pick the popular "react-native-static-server" and here's how we set up the server:

```
// App.js
this.server = new StaticServer(8080, Platform.OS == "android" ? RNFS.DocumentDirectoryPath + "/www":RNFS.MainBundlePath + "/www"); // Android doesn’t have a "MainBundle" directory so it requires a different path
```

Now let's add our HTMLs and models as assets to the platforms! Add your resources to "/www" which is where the static server looks for resources by default. For iOS open the project in "./ios" on Xcode and add the "./www" folder to the project. For Android project you can simply add the following code to your "app/build.gradle":

```
// app/build.gradle
android {
...
    sourceSets { main { assets.srcDirs = ['src/main/assets', '../../assets'] } }
}
```

One more thing to note is that we need to add "react-native-fs" so we can move our files and get the directories where our content is located, since we are not exactly allowed serve assets from the asset directory and we hence need a separate directory to serve our contents, plus Android doesn’t have a MainBundle directory so it requires a different path...To overcome this we simply do:

```
// App.js
if (Platform.OS == "android") {
      await RNFS.mkdir(RNFS.DocumentDirectoryPath + "/www");
      const files = ["www/index.html"];  //be sure to iterate each files individually here
      await files.forEach(async file => {
        await RNFS.copyFileAssets(file, RNFS.DocumentDirectoryPath + "/" + file);
      })
    }
```

For more details see our code sample here: https://github.com/dukedhx/viewer-reason-react-native/blob/master/App.js

## NativeScript

[NativeScript](https://docs.nativescript.org/) is a framework for building truly native mobile apps with Angular, Vue.js, TypeScript, or JavaScript, enabling one to use a complete stack of cross-platform APIs to write your application code or directly access all platform-specific native APIs using JavaScript only. It's maintained by Telerik, which might ring a bell if you work extensively with .NET, but it appears to have lost a bit of momentum in recent years due to the rise of other popular frameworks...

To slip in a Viewer instance all we need to do is just to set up your component as you would for your Cordova/Capacitor hybrid apps:

```
// ./src/app/home/home.component.html
<WebView [src]="webViewSrc"></WebView>

// ./src/app/home/home.component.ts
export class HomeComponent implements OnInit {

    private webViewSrc = `<!DOCTYPE html> ...
```

And NativeScript, similar to Expo, offers an impressive (albeit sensibly defective in quite a few areas) tool set that comes with a CLI tool `tns` (Telerik NativeScript obviously), including a package manager, an online playground and iOS/Android apps to preview your work on devices etc (see [here](https://docs.nativescript.org/tooling/docs-cli/project/testing/preview) for details). The toolset also take cares of building and publishing to different devices and distribution platforms ([here](https://docs.nativescript.org/tooling/docs-cli/publishing/appstore-upload) for details):

```
# tns preview
# tns build ios/android
# tns publish ios/android
```

So pick your favorite tools and platforms and we are all set:

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/Untitled%203_2.PNG)

To set up and run the sample refer to instructions in the README: https://github.com/dukedhx/viewer-nativescript-angular

Still plenty of more to the show like support for Vue (see [here](https://docs.nativescript.org/vuejs)) and NativeScript Sidekick - a GUI client tool to manage NativeScript projects, see more details [here](https://docs.nativescript.org/sidekick/getting-started/overview).
