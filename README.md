# Electric Book app template

This is a boilerplate for creating Electric Book apps with Cordova.

This template is integrated into the [Electric Book template](http://github.com/electricbookworks/electric-book), where it is used to create Android and iOS apps. This repository stores the full boilerplate for other potential builds, and, below, more detailed usage documentation.

# Prerequisites

## Basic tools

You need to have [Node.js](https://nodejs.org) installed (including npm, which it does by default) so that npm can be run from the command line.

Once you have Node.js, you can install Cordova.

## Installing Cordova

The [Cordova](https://cordova.apache.org/) packages install easily using npm with:

`npm install -g cordova` on Windows, or

`sudo npm install -g cordova` on OSX and \*nix.

After this, just run `cordova` on the command line for its help text.

## Android

The requirements for android builds are listed [here](https://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html#requirements-and-support) but amount to installing [Android Studio](https://developer.android.com/studio/index.html). By default the required SDKs and Android versions should be available.

## iOS

The requirements to build an iOS application are listed [here](https://cordova.apache.org/docs/en/latest/guide/platforms/ios/index.html). They amount to:

* Install XCode from the App Store, available by searching for "Xcode" in the App Store. (XCode can equivalently be downloded from Apple Developer Downloads if you have a developer licence.)
* Install the command-line tools for XCode from the command line using: `xcode-select --install`.
* Install the deployment tools with `npm install -g ios-deploy`. [If you have trouble](https://github.com/phonegap/ios-deploy/issues/109#issuecomment-92589783), you may need to be heavy handed and run `sudo npm install --global --unsafe-perm ios-deploy`.

# Creating a new app

## Set Up

### Structure

Start with a copy of this repository either by forking or duplicating it.

### Copy the app files

Assuming you're using the Electric Book template, build your app files with it. Alternatively, use any well-structured, self-contained web HTML.

Copy these files into the `bookapp/www` directory. All HTML resources must be within this directory.

### Edit config.xml

The key setup file is `config.xml` in the `bookapp` directory. Open this file in a text editor. The following fields need to be set up in this file:

* **The bundle ID:** The bundle ID is specified in the first line of the config XML under widget as id. This is the unique identifier for the book and must be unique across all play stores. The convention for this is to always use the reverse of a URL that you own. For example, if you own the url `fireandlion.co.za` you might use the bundle ID `za.co.fireandlion.MyBookTitle`.

  The Bundle ID defines a specific app on the store, so you can't change it after you set it (because then it would be a different app, not a new version of the same app).

* **Version number:** Immediately after the bundle ID is a version number. Every new version that you want to push to a store requires a unique version number for distribution. If the store is refusing to take a new update of the app it is probably because this number has not been updated.
* **Name:** The title of the application as it will appear on the store below its icon.
* **Description:** A longer description of the app for some operating-system displays. This is rarely seen; it is not the advertising copy that appears on stores: those descriptions can be set up on a store-by-store basis online.

### Icons

Replace the `icon.png` file in `bookapp/res` with a square PNG image that is 120 × 120 pixels (iOS builds can fail if it is not exactly this size). The system will automatically build all other icons in the directory.

## Doing the Build

### Build for Android

From the command prompt go into the bookapp directory and run:

```
cordova build android
```

This process will create an `.apk` file that can be directly installed on a device. If you have a mobile device attached by USB (and set up as a debuggable development device, which can be configured in settings, and is sometimes hidden on some devices) you should be able to run the build directly on the device by running:

```
cordova run android --device
```

#### Signing the app

Finally, to work on the Play Store, the application much be digitally signed. This means generating a signature on a local machine.

This keystore file that you create is critical for proving your identity to the Play Store, and if you lose it you are really in trouble! So you need to keep this keystore *very safe* and *very private* – do not commit it to your repository.

Use this command from the base directory to generate a keystore:

```
keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-alias
```

Where you should:

* replace `my-release-key.jks` with the name of the keystore it will create, and 
* `my-alias` with a specific alias to a key within the keystore.

This process will prompt you to create a password, which you need to remember.

Now edit the `build.json` file in your main directory and correctly set the path to the keystore, the passwords and the alias.

After you have completed these steps, you can build a signed release copy ready for the store by running:

```
cordova build android --release
```

### Build for iOS

#### First-time refresh

After you have changed the bundle ID, the iOS components need to be scratch configured again. This process only need to be done for iOS
and only once after you set up the bundle ID.

From the command prompt go into the bookapp directory and run these three commands in order:

```
cordova plugin save
cordova platform rm ios
cordova platform add ios
```

(On Windows, you can also do this by just running the batch file `refreshios.bat`.)

This process will reconfigure the iOS project correctly.

#### Build

From the command prompt go into the bookapp directory and run:

```
cordova build ios
```

This process builds a full XCode application. Open this output as a project in XCode. From there, you can:

* build it for the iStore
* send it directly to your USB-connected test device, or
* open it in the iOS simulator in XCode.
