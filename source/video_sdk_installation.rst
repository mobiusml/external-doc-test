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
  
In case you are experiencing trouble installing the SDK please take a look at our exception handling section. 
