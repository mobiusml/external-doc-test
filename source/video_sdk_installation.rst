SDK Installation
-------------------

This is a version of the Mobius Vision SDK that uses Qualcomm's Snapdragon Neural Processing Engine SDK (SNPE) to boost performance by leveraging the GPU/DSP of Snapdragon chips. This version supports only the ``GPU``.


Installation
================

The SDK is delivered as an AAR. Adding the AAR into an Android Studio project can be done by simply opening the project structure and add a new module *"Import .JAR or .AAR Package"* or adding the aar file to a directory (for example 'libs') and use flatDir in your build.gradle :

.. code-block:: gradle

     allprojects {
       repositories {
           ...
           flatDir{ dirs 'libs' } //here the aar file is in the libs directory
       }
     }
     dependencies {
     ...
     compile(name:'mobiusvisionsdk', ext:'aar')
     }
     
     
Loading the SDK
================

To load the SDK call the load function :

.. code-block:: Java

     public class MainActivity extends AppCompatActivity {
       ...
       MobiusSDK.load(this.getApplication());
       //Check if the SDK was correctly loaded
       if (!MobiusSDK.isOperational()){...}
       ...
     }
  
Please consider using an AsyncTask to load the SDK on start-up in your app.
  
  
Common Problems
===================

* If the SDK can't load, it is most likely because of OpenCL. Please make sure that your device uses a Snapdragon chip and that it can access *libOpenCL.so* (usually the .so should be in the public lib list : /system/etc/public.libraries.txt)
* If you get the following error when building:

.. code-block:: gradle

     Error:error: the input file ... libsnpe_dsp_skel.so' has no sections
  
Try to add the following in your gradle file:
 
.. code-block:: gradle

     buildTypes {
     ...
       packagingOptions {
           doNotStrip "**/*/*.so"
       }
     }
  
If you're using Proguard or DexGuard, please add this rule in your rules config file:

.. code-block:: proguard

     -keep class com.qualcomm.**{*;}
     -keep class ml.mobius.**{*;}