Installation
==================

Before you can use the Mobius mobile SDK, the installation has to be completed as explained here.


Requirements for the Mobius mobile SDK
-----------------------------------------

In order to run functions of the SDK on a mobile device, there are some technical requirements on the hardware and the software.

Full requirements list:

*   architectures: "armeabi-v7a", "arm64-v8a"
*   Android 7.0 or higher
*   For Python 2, version 2.7
*   For Pyhton 3, version 3.3 or higher

Installation procedure
-------------------------

The SDK is delivered as an AAR. In order to use functions of the SDK, the AAR has to be added into an Android Studio project.  


*   Step1: open the project structure, select option to add a new module "Import .JAR or .AAR Package" and select the Mobius Vision aar file. 
*   move the AAR file to a directory (for example 'libs') and 
*   use flatDir in your build.gradle :

::

  // Code for option B
  allprojects {
      repositories {
          ...
          flatDir{ dirs 'libs' } //in this example the aar file is in the libs directory
      }
  }
  
::

  dependencies {
  ...
  compile(name:'mobius-vision', ext:'aar')
  }


That's it! You can test the SDK with instructions from the next section. 
