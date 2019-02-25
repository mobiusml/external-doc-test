Installation
==================

Before you can use the Mobius mobile SDK, the installation has to be completed as explained here.


Mobius Vision SDK versions
---------------------------
Generally there are two basic versions of the SDK for Android. One version that can only use the CPU of the phone and the GPU version that can also use the GPU of Snapdragon chips. 

.. note::

  As video processing is resource intensive, it is only available with a GPU version. 


Requirements for the CPU version
----------------------------------

In order to run functions of the SDK on a mobile device, there are some technical requirements on the hardware and the software.

Full requirements list:

*   architectures: "armeabi-v7a", "arm64-v8a"
*   Android 7.0 or higher
*   For Python 2, version 2.7
*   For Pyhton 3, version 3.3 or higher

Additional requirements for the GPU version
---------------------------------------------
The GPU version of the Mobius Vision SDK uses the Qualcomm Snapdragon Neural Processing Engine SDK (SNPE) to boost performance by leveraging the GPU/DSP of Snapdragon chips. This version supports only the ``GPU``.


Installation procedure
-------------------------
The SDK is delivered as an AAR. In order to use functions of the SDK, the AAR has to be added into an Android Studio project.  


*   open the project structure, select option to add a new module "Import .JAR or .AAR Package" and select the Mobius Vision aar file, 
*   move the AAR file to a directory (for example 'libs') and 
*   use flatDir in your build.gradle :

.. code-block:: gradle

  allprojects {
      repositories {
          ...
          flatDir{ dirs 'libs' } //in this example the aar file is in the libs directory
      }
  }
  
.. code-block:: gradle

  dependencies {
  ...
  compile(name:'mobiusvisionsdk', ext:'aar')
  }


That's it! You can test the SDK with instructions from the next section. 
