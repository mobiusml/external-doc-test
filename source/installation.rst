Installation
==================

Before you can use the Mobius mobile SDK, the installation has to be completed as explained here.


Requirements for the Mobius mobile SDK
-----------------------------------------

We have some technical requirements on the hardware and the software for using the SDK.

There are the following requirements:

*   architectures "armeabi-v7a", "arm64-v8a", "x86", "x86_64"
*   Android > 7.0
*   Python 2.7 (or 3.3+ for Python 3)

Installation procedure
-------------------------

The SDK is delivered as an AAR. In order to use functions of the SDK, the AAR has to be added into an Android Studio project.  

We recommend to choose either of the following methods:

*   A: open the project structure, select option to add a new module "Import .JAR or .AAR Package" and select the Mobius Vision aar file. 
*   B: move the AAR file to a directory (for example 'libs') and use flatDir in your build.gradle :

::

  // Code for option B
  allprojects {
      repositories {
          ...
          flatDir{ dirs 'libs' } //in this example the aar file is in the libs directory
      }
  }

  dependencies {
  ...
  compile(name:'mobius-vision', ext:'aar')
  }


That's it! You can test the SDK with instructions from the next section. 
