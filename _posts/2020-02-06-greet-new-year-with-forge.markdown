---
layout: post
title: Greet the Year of the Mouse with your first Ionic/Capacitor cross-platform Viewer app - in Angular & TypeScript with Managed Viewer Dependencies
date: 2020-02-06 13:32:20 +0300
description: Today we are going to explore building cross platform Forge Viewer apps with Ionic & Capacitor in Angular and TypeScript.  # Add post description (optional)
img: 2055D4FB-D4E1-436E-993A-FC715B559E02.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: []
---
In this final episode of our cross platform native Forge app series (see previous installments [here](https://forge.autodesk.com/author/bryan-huang)) today we are going to explore building cross platform Forge Viewer apps with Ionic & Capacitor in Angular and TypeScript (see other TypeScript resources [here](https://forge.autodesk.com/blog/all-definitely-defined-typescript-definitions-updates-and-react-typescript-sample)). And on a side note the [Year of the Mouse](https://en.wikipedia.org/wiki/The_Year_of_the_Mouse) is right upon us and hence the festive title and cover of this blog! As such our team in China will go on national holiday next week - if you have any queries being handled by one of them and require urgent attention please send another separate email to forge.help@autodesk.com so somebody else can pick up where we left off...

Code sample: https://github.com/dukedhx/viewer-ioniccapacitor-angular

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/2055D4FB-D4E1-436E-993A-FC715B559E02.png)

## Why Capacitor & Ionic

Capacitor is a cross-platform app runtime that makes it easy to build web apps that run natively on iOS, Android, Electron (see [here](https://forge.autodesk.com/blog/standalone-executable-project-of-forge-viewer-on-pc-by-electron) to develop Forge with Electron), and the web. We call these apps "Native Progressive Web Apps" and they represent the next evolution beyond Hybrid apps which are what we used to scrape by with older frameworks like Cordova etc (see [here](https://forge.autodesk.com/blog/standalone-app-of-forge-viewer-on-ios-by-cordova) about developing Viewer apps with Cordova). Capacitor provides a consistent, web-focused set of APIs that enable an app to stay as close to web-standards as possible, while accessing rich native device features on platforms that support them. Adding native functionality is easy with a simple Plugin API for Swift on iOS, Java on Android, and JavaScript for the web. Capacitor is a spiritual successor to Apache Cordova and Adobe PhoneGap, with inspiration from other popular cross-platform tools like React Native and Turbolinks, and can be integrated with Ionic/Angular or React.

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/C926AE82-D294-4F67-93DA-4BA95A07853E.jpeg)

## Getting Started

To create a refresh Ionic/Capacitor:

- Install Node.js/Ionic
- Create the project with the Capacitor flag:

```
ionic start appName --capacitor
```

To Navigate to your project and run:

```
ionic integrations enable capacitor
```

And to migrate from your existing Cordova or PhoneGap projects see here: https://capacitor.ionicframework.com/docs/cordova

So much for the background and now without further ado let's have a look at our sample.

## Viewer Dependencies and Life Cycle

In our sample there's one home view with one Viewer component inside that the app would route to by default. With our Forge dependency loader service this can scale easily - you can request to load the Viewer's dependencies dynamically as needed and as the user navigates back and forth or your Viewer instances grow in number your code signature remains the same and the dependencies would always get loaded once, with "thenables" available when they completes loading to spin up your Viewer workflows:

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/95FEE793-61CE-4CCB-9A6B-08BCE909AA40.jpeg)

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/Untitled_7.png)

## Bridging Viewer and Native APIs

For Viewer to interact with native APIs it is quite straightforward - which is one of the perks of going along the Hybrid app lines. Capacitor includes a number of Native APIs that are available to all Capacitor apps. These can be thought of as Capacitor "core plugins," and they make it easy to access commonly needed functionality on each platform, for example:

```
const { Browser } = Plugins;
//...
async openBrowser() {
  // On iOS, for example, open the URL in SFSafariViewController (the in-app browser)
  await Browser.open({ url: "https://ionicframework.com" });
}
```

The community has built a number of Capacitor plugins to add functionality to your app. You may find a curated collection of plugins here: https://github.com/ionic-team/capacitor/edit/master/site/docs-md/community/plugins.md

## Running the Sample

- Code the sample: https://github.com/dukedhx/viewer-ioniccapacitor-angular
- Copy your model's SVF files to `src/assets` and edit the load path accordingly in `src/app/viewer/forge.viewer.ts` (or feel free to change the code to load from remote URL or Forge services if preferred)
- Navigate to the project directory and run:

```
npm install // install Node.js dependencies
npm run build // build the Ionic app, you may debug the app in your browser with `npm start` or `npx ng serve`
npx cap add ios // if building to iOS
npx cap add android // if building to Android
```

### iOS

```
npx cap open ios //or open the project (or workspace) in the `ios` folder on Xcode
```

Build and run on emulator or connected device:

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/14EF38AC-0E9F-417C-92B8-92844D7286B0.jpeg)

### Android

```
npx cap open android //or open the project in the `Android` directory with your favorite IDE
```

Build the project with Gradle and debug/run on emulator or connected device:

![sb](https://flint-prodcms-forge.s3.amazonaws.com/prod/s3fs-public/inline-images/Screenshot_20200120-152630_0.png)



## Branding and Publishing

\- To brand your app and customize the icons, splash etc you will need to edit the platform specific input files respectively - follow [here](https://enappd.com/blog/icon-splash-in-ionic-react-capacitor-apps/114/) and [here](https://github.com/ionic-team/capacitor/issues/854)
\- Publishing to iOS App Store: https://www.joshmorony.com/deploying-capacitor-applications-to-ios-development-distribution/
\- Publishing to Google Play Store: https://www.joshmorony.com/deploying-capacitor-applications-to-android-development-distribution/
