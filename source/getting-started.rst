Getting Started and Prediction
================================


Loading the SDK
------------------

To load the SDK, create one instance of the :java:`MobiusSDK` class inside your activity:

::

  public class MainActivity extends AppCompatActivity {
    ...
    MobiusSDK.load(this.getAssets());
    //Check if the SDK was correctly loaded
    if (!MobiusSDK.isOperational()){...}
    ...
  }



Prediction
-----------


The predict function takes as input an image and returns a result object. The result objects vary according to the use case. When using the keywording model, a :java:`KeywordingResult` object will be returned. 




Prediction with Pre-trained Modules
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

|sdk_name| is shipped with two modules that were trained on our large image dataset.
These *pre-trained modules* can be used to obtain predictions for a wide range of applications.

Illustration of the prediction feature for keywording and aesthetics:

.. image::
   data/prediction_public_models.png
   :height: 400 px
   :width: 600 px
   :align: left


**Keywording**

This is an example for prediction with our standard keywording model:

::

  KeywordingResult keywordingResult = MobiusSDK.predictKeywords(bitmap);
  //Returns thresholded results with default threshold
  ArrayList<Keyword> keywords = keywordingResult.getThresholdedResult();

From the returned :java:`KeywordingResult` object an ArrayList can be extracted with all keywords that were detected in the image above the default threshold. 

.. note::

  The default threshold is 0.55. For a keyword to be returned, the model has to have at least 55% confidence that this keyword is present in the image. It is possible to set a custom threshold by passing a float value to the function :java:`getThresholdedResult(custom_value_float)`. However, this float value should be greater than 0 and smaller than 1. 

You can get a particular label(String) or confidence(Float) from the list as follows :

::

  String label = keywords.get(i).getLabel(); 
  Float confidence = keywords.get(i).getConfidence(); //confidence of the model in the keyword

.. note::

  For some simple images, the keywording module might only recognise a small number of matching keywords.
  However, in cluttered scenes, there might be a long list of matching keywords. Depending of the image, the number
  of returned keywords might vary. If no keywords are returned this might mean that the threshold was too high and there was no keyword where the model was sufficiently confident to present as result.


There is also a function to obtain the top k keywords ordered descending according to the confidence. This function returns
the same number of keywords in every call.

::

  //Returns top-k results with default k value of 15
  ArrayList<Keyword> keywords = keywordingResult.getTopK();
  //Returns top-k results with custom k value
  ArrayList<Keyword> keywords = keywordingResult.getTopK(20);

.. note::

  It is theoretically possible to select an integer value between 1 and the total number of keywords in the model. However, large values of k are likely to result in a large number of keywords will low confidence and therefore it is more likely to see wrong labels. The value of k should be carefully adjusted according to the use case and image data. 

**Aesthetics**

Prediction with the aesthetics module works in a similar manner as the keywording module. Also in this case a specific :java:`AestheticsResult` object is returned from the predictAesthetics function of the |sdk_name|. 

::

  AestheticsResult aestheticsResult = MobiusSDK.predictAesthetics(bitmap);
  float aestheticsScore = aestheticsResult.getScore(); //Returns normalized aesthetics score

.. note::

  The aesthetics score is normalized, that means that it will always be a value between 0 and 1 which only depends on the quality of the single input image to this function. 


Prediction on features
-------------------------


.. note::

  For both Keywording and Aesthetics, you can do predictions on the features as well instead of the Bitmap image. This is useful when the features are cached in the app to prevent re-processing the images:

::

  KeywordingResult keywordingResult = MobiusSDK.predictKeywords(float[] keywordingFeatures);
  AestheticsResult aestheticsResult = MobiusSDK.predictAesthetics(float[] aestheticsFeatures);


Prediction with a customised model
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This SDK also offers the option to train custom models on top of our pre-trained models. If a custom model has been just trained or a trained model has been loaded the procedure is slightly different compared to the previous sections.

In this case, you need to specify a key for every trained |model|. If a customized |model| has been trained already:

::

  //Loading a custom  model for example (here called mainCustomModelFileName) can done as follows
  String path = this.getFilesDir().getPath() + "/" + mainCustomModelFileName;
  try{
      MobiusSDK.loadCustomModel(customModelKey, path);} //A new model is automatically created in the SDK
  catch (FailedLoadingModelException e) {...}
  //if the custom model model was already trained
  if(MobiusSDK.CustomModelisTrained()){
      float predictedScore = MobiusSDK.predictCustomModel(customModelKey, bitmap).getScore();}

You can also classify by using the default threshold or a custom threshold :

::

  Boolean prediction = MobiusSDK.predictCustomModel(customModelKey, bitmap).classify(); //default threshold
  Boolean prediction = MobiusSDK.predictCustomModel(customModelKey, bitmap).classify(0.7f); //custom threshold

If the features are cached, custom model prediction can be much faster by calling predictCustomModel on the features instead of the bitmap :

::

  float predictedScore = MobiusSDK.predictCustomModel(customModelKey, float[] features).getScore();







Prediction with large number of images
-----------------------------------------

For many applications there might be a need to process many images. Prediction
is much faster than training a model, yet it is computationally expensive.

.. todo::

  put number or chart here

Here is one example for prediction:

.. warning::

  Prediction is time consuming! It's recommended to run predictions in a separate thread.

.. todo::

  put code here
