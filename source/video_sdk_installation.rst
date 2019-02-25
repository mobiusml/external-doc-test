Getting started
-------------------

Before being able to use the SDK in an Android project, you need to load the SDK first. 
     
     
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
