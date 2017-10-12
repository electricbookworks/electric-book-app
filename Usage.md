# Prerequisites
## Basic tools
* npm and node - you need to have both node.js and npm installed so that they can be run from the command line

## Installing Cordova
The cordova packages install easily using npm with:
```npm install -g cordova``` (windows) OR
```sudo npm install -g cordova``` (osX+*nix)

After this installation just running 'cordova' on the command line should give you lots a help text.

### Android
The requirements for android builds are listed here https://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html#requirements-and-support
but amount to
* Install Android Studio from https://developer.android.com/studio/index.html ; by default the required SDK's and android versions should be available.

### iOS
The requirements to build an iOS application are listed here https://cordova.apache.org/docs/en/latest/guide/platforms/ios/index.html but amount to
* Install XCode from the App Store, available by searching for "Xcode" in the App Store application. (XCode can equivalently be downloded
from Apple Developer Downloads if you have a developer licence)
* install the command line tools for xcode from the command line using:
```xcode-select --install```
* Install the deployment tools with (this tool is not required for the process and doesn't install on older OSX versions - so if it
gives trouble installing just forget about it):
```npm install -g ios-deploy```

# Creating a new book

## Set Up

### Structure
Start with a copy of this repository either by forking or duplicating it.

### Copy the web files
Your web files must be copied into the bookapp/www directory. All HTML5 resources must be within this directory.

### Editing Config XML
The key set up file is config.xml in the bookapp directory, open this file in a text editor. The following fields need to be set up in this file:
* The bundle ID: This is the unique identifier for the book and must be unique across all play stores - the
strategy to achieve this is to always lead with the use the reverse URL that you own.
For example if you owned the url www.fireandlion.co.za - you can might use the bundle ID za.co.fireandlion.MyBookTitle
Bundle ID define what makes a specific app on the store so you can;t change these after you set them (because then it would
be a different app not a new version of the same app. The bundle id is specified in the first line of the config XML under widget as id.
* Version Number: Immediately after the bundle id is a version number - every new version that you want to push to a store
requires a unique version number for distribution - if the store is refusing to take a new update of the app it is probably
because this number has not been updated.
* Name: The title of the application as it will appear below its icon.
* Description: A longer description of the app for some operating system displays (rarely seen) - this is not the
 'advertising' copy that appears on the store sites; those can be set up on a store by store basis online.

### Icons
You should replace the icon.png file in bookapp/res with a sqaure png image at leasr 128 x 128 pixels. The system will automatically
build all other icons in the directory.


## Doing the Build

### First time refresh for ios only
After you have changed the bundle ID the ios components need to be scratch configured again - this process only need to be done for ios
and only once after you set up the bundle id.
From the command prompt go into the bookapp directory and run the batch file: ```refreshios.bat```
This process will reconfigure the ios project correctly.

### Build for Android
From the command prompt go into the bookapp directory and run:
```cordova build android```

This process will create a .APK file that can be directly installed on a device. If you have a mobile device attached by USB
(and set up as a debuggable development device - this can be configured in settings and it sometimes hidden on some devices) you should
be able to run the build directly on the device by going:
cordova run android --device

### Build for iOS
From the command prompt go into the bookapp directory and run:
```cordova build ios```

This process builds a full xcode application. You can open this output as a project in XCode and build it for the iStore/send it directly to your test
device/ open it in the iOS simulator to see it.
