---
layout: post
title: Kick start 2020 with cross-platform Viewer apps in Flutter!
date: 2020-01-16 13:32:20 +0300
description: The wheel never stops in the developer's world and Flutter is gaining huge momentum for recent years so let's see how one should build Viewer Apps with Flutter today and our demo would cover how to load models locally from within client  # Add post description (optional)
img: weimingming.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: []
---
The wheel never stops in the developer's world and Flutter is gaining huge momentum for recent years so let's see how one should build Viewer Apps with Flutter today and our demo would cover how to load models locally from within client! In fairness it's not exactly as straight forward to bundle Viewer models with the apps and we had addressed this topic [here](https://forge.autodesk.com/blog/fast-track-your-react-native-forge-app-expo-sdk-part-ii), [here](https://forge.autodesk.com/blog/running-forge-viewer-react-native-offline) and [here](https://forge.autodesk.com/blog/offline-viewing-android) previously. But with the advent of such a massive game changer like Flutter there goes all the pain and anguish so let the fun begin right away!

- Sample Project: **https://github.com/dukedhx/viewer-flutter-dart**

## **Why Flutter or Dart?**

Flutter is Google's UI toolkit for crafting beautiful, natively compiled applications for mobile, web, and desktop from a single codebase, AOT (Ahead Of Time) compiled to fast, predictable, native code, which allows almost all of Flutter to be written in Dart - a sound typed, client-optimized, object-oriented, class defined, garbage-collected language using a C -style syntax for apps on multiple platforms, server and clients alike. This not only makes Flutter fast, virtually everything (including all the widgets) can be customized. Let's see how Dart stands out as an emerging apple in the programmers' eyes:

- Developers would find Dart particularly easy to learn because it has features that are familiar to users of both static and dynamic languages and Flutter’s package/project  management paradigm is extremely intuitive for Node/Ruby/.NET/Java (Gradle etc.) programmers!
- Dart can be efficiently compiled AOT or JIT, interpreted, or transpiled into other languages. Not only is Dart compilation and execution unusually flexible, it is especially fast.
- Dart by default is AOT but can also be JIT (Just In Time) compiled for exceptionally fast development cycles and game-changing workflow (including Flutter’s popular sub-second stateful hot reload).
- Dart also provides a standalone VM that uses the Dart language itself as its intermediate language (essentially acting like an interpreter).
- Dart makes it easier to create smooth animations and transitions that run at 60fps. Dart can do object allocation and garbage collection without locks.
- Dart, like JavaScript, is single threaded, which means it does not allow preemption at all. Instead, threads explicitly yield (using async/await, Futures, or Streams). This gives the developer more control over execution. Single threading helps the developer ensure that critical functions (including animations and transitions) are executed to completion, without preemption. This is often a big advantage not just for user interfaces, but for other client-server code.
- Like JavaScript, Dart avoids preemptive scheduling and shared memory (and thus locks). Because Flutter apps are compiled to native code, they do not require a slow bridge between realms (e.g., JavaScript to native). They also start up much faster
- Dart allows Flutter to avoid the need for a separate declarative layout language like JSX or XML, or separate visual interface builders, because Dart’s declarative, programmatic layout is easy to read and visualize. And with all the layout in one language and in one place, it is easy for Flutter to provide advanced tooling that makes layout a snap, consider the following code:

```
new Center(child:
  new Column(children: [
    new Text('Hello, World!'),
    new Icon(Icons.star, color: Colors.green),
  ])
)
```

And it'd build to the following UI:

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/178B3FFB-F32C-4433-AE68-BE19A68AFB91.jpeg)

**As this article is being published, Dart 2 is being released. Dart 2 is focused on improving the experience of building client apps, including developer velocity, improved developer tooling, and type safety. For example, Dart 2 features a sound type system and type inferencing.**

**Dart 2 also makes the new keyword optional. This means that it is possible to describe many Flutter views without using any keywords at all, making them less cluttered and easier to read. For example:**

```
Widget build(BuildContext context) =>
  Center(child:
    Column(children: [
      Text('Hello, World!'),
      Icon(Icons.star, color: Colors.green),
    ])
  )
```

## Viewer <> Flutter

Without further ado let's see how Viewer clicks with Flutter - of course we'd still need to fall back as always to WebView to load Viewer scripts as well as the models since Viewer is yet to be implemented in any native form. However we'd be able to load models as bundled resources by starting a local http server on client and it gets even better: we'd be able to build to iOS, Android and even web targets using exactly the same Dart code and Flutter project/dependencies settings! Now let's walk through the dependencies as well the structure of our sample app here:

- Jaguar: client side http server
- Jaguar Flutter asset:  serve Flutter assets
- Flutter WebView plugin: WebView module for iOS, Android and Web  
- Flutter icon plugin: custom icons and splash screens
- Viewer assets: SVF model and html files in the asset folders to be served as bundled resources

## Setup and Run

**Prerequisites**

- [Flutter](https://flutter.dev/docs/get-started/editor)
- [iOS](https://flutter.dev/docs/get-started/editor)
- [Android](https://flutter.dev/docs/get-started/editor), when integrating with existing projects be sure to migrate to [Androidx](https://developer.android.com/jetpack/androidx/) since it’s required by the “flutter_webview_plugin”.

**Project Setup**

- Clone project: https://github.com/dukedhx/viewer-flutter-dart
- [Set up your favorite IDE with Flutter](https://flutter.dev/docs/get-started/editor)
- Put your SVF model files to the “assets” folder and change the URL to point to the SVF entry file accordingly (see here to download the SVF files from Forge)
- Verify Flutter installation and readiness: `flutter doctor`
- Set up pods if you haven’t: `pod setup`
- Navigate to project folder and set up the dependencies: `flutter get pub`



**Build and Run:**

- Run on emulators: `flutter run`
- Build to platform targets: `flutter build ios` / `flutter build apk`

And enjoy:

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/FAFC2E4B-9632-4358-BECD-A2543BC4E9C2.jpeg)

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/CBF790B2-289E-4818-98C3-D4C96EEAC686.jpeg)

## What Next?

- Flutter can help publish your apps to platforms as well - see [here](https://flutter.dev/docs/deployment/flavors)
- Looks into the Flutter WebView plugin’s API to communicate with Viewer and other components inside

Okey dokey - that's all we had time for today and we will be back to try things out with Ionic Capacitor to wrap our quest into native dev with Viewer. Until next time!
