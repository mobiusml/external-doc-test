Installation
==================

Before you can use the Mobius mobile SDK, you have to follow a few steps as explained here.


Requirements for the Mobius mobile SDK
-----------------------------------------

Due to the advanced models, we have some requirements for using the mobile SDK.
There are Requirements on the hardware and the software.

There are the following requirements:

*   Android > 7.0
*   architectures "armeabi-v7a", "arm64-v8a", "x86", "x86_64"
*   Python 2.7 (or 3.3+ for Python 3)

Installation procedure
-------------------------

The SDK is delivered as an AAR. Adding the AAR into an Android Studio project can
be done by simply opening the project structure and add a new module "Import .JAR or
.AAR Package", or adding the aar file to a directory (for example 'libs') and use flatDir in your build.gradle :

::

  allprojects {
      repositories {
          ...
          flatDir{ dirs 'libs' } //here the aar file is in the libs directory
      }
  }

  dependencies {
  ...
  compile(name:'vision-debug', ext:'aar')
  }
