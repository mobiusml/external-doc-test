Getting Started and Prediction
================================


Loading the SDK
------------------

To load the SDK, create one instance of the :java:`MobiusSDK` class inside your activity:

::

  public class MainActivity extends AppCompatActivity {
      MobiusSDK mobiusSDKInstance;
      ...
      mobiusSDKInstance = new MobiusSDK(this.getAssets());
      //Check if the SDK was correctly loaded
      if (!mobiusSDKInstance.isValid()){...}
      ...
  }


Prediction
-----------


The predict function takes as input an image and returns a result object. This function
returns a prediction

.. image::
   data/function_overview_prediction.png
   :height: 250 px
   :width: 500 px
   :align: center



Prediction with Pre-trained Modules
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

|sdk_name| is shipped with two modules that were trained on our large image dataset.
These *pre-trained modules* can be used to obtain predictions for a wide range of applications.

Illustration of the prediction feature for keywording and aesthetics:

.. image::
   data/prediction.png
   :height: 400 px
   :width: 600 px
   :align: left


**Keywording**

This is an example for prediction:

::

  KeywordingResult keywordingResult = mobiusSDKInstance.predictKeywords(bitmap);
  //Returns thresholded results with default threshold
  ArrayList<Keyword> keywords = keywordingResult.getThresholdedResult();

The list of returned results is in the form of Keyword object. You can get the label(String)/confidence(Float) as follows :

::

  String label = keywords.get(i).getLabel(); //return the label
  Float confidence = keywords.get(i).getConfidence(); //returns the confidence

In this case, all keywords with a confidence above a certain threshold are returned.
For some simple images, the keywording module might only recognise a small number of matching keywords.
However, in cluttered scenes, there might be a long list of matching keywords. Depending of the image, the number
of returned keywords might vary.

.. todo::

  what are the concrete default values? why did we pick these?

There is an additonal function to obtain the top k keywords. This function returns
the same number of keywords in every call.

::

  //Returns top-k results with default k value
  ArrayList<Keyword> keywords = keywordingResult.getTopK();
  //Returns top-k results with custom k value
  ArrayList<Keyword> keywords = keywordingResult.getTopK(20);


**Aesthetics**

Prediction with the aesthetics module works in a similar fashion.

::

  AestheticsResult aestheticsResult = mobiusSDKInstance.predictAesthetics(bitmap);
  float aestheticsScore = aestheticsResult.getScore(); //Returns normalized aesthetics score

.. todo::

  wondering if it makes sense to swap aesthetics and keywording since keywording is more complex


**Prediction on features**

.. note::

  For both Keywording and Aesthetics, you can do predictions on the features as well instead of the Bitmap image. This is useful when the features are cached in the app to prevent re-processing the images:

::

  KeywordingResult keywordingResult = mobiusSDKInstance.predictKeywords(float[] keywordingFeatures);
  AestheticsResult aestheticsResult = mobiusSDKInstance.predictAesthetics(float[] aestheticsFeatures);



Prediction with a customised model
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If a customisation module was already trained or has been loaded the procedure is slightly different.

In this case, you need to specify a key for every trained |model|. If a customized |model| has been trained already:

::

  //Loading a custom  model for example (here called mainCustomModelFileName) can done as follows
  String path = this.getFilesDir().getPath() + "/" + mainCustomModelFileName;
  try{
      mobiusSDKInstance.loadCustomModel(customModelKey, path);} //A new model is automatically created in the SDK
  catch (FailedLoadingModelException e) {...}
  //if the custom model model was already trained
  if(mobiusSDKInstance.CustomModelisTrained()){
      float predictedScore = mobiusSDKInstance.predictCustomModel(customModelKey, bitmap).getScore();}

You can also classify by using the default threshold or a custom threshold :

::

  Boolean prediction = mobiusSDKInstance.predictCustomModel(customModelKey, bitmap).classify(); //default threshold
  Boolean prediction = mobiusSDKInstance.predictCustomModel(customModelKey, bitmap).classify(0.7f); //custom threshold

If the features are cached, custom model prediction can be much faster by calling predictCustomModel on the features instead of the bitmap :

::

  float predictedScore = mobiusSDKInstance.predictCustomModel(customModelKey, float[] features).getScore();


.. note::

  the SDK does by default a slight cropping which improves keywording predictions, this cropping is set inside the resizing function in BitmapUtils but can be turned off.

Prediction with large number of images
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Please note that prediction is time consuming. It's recommended to run predictions
in a separate thread. There is one example in the section of on device training.

.. todo::

  put reference here - maybe little note / warning
