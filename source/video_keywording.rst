Video Tagging
--------------

When tagging videos, one has to decide on a temporal segment that should be tagged. 
The SDK provides two different ways to perform video keywording; one that uses a fixed time lapse to get sequences of equal duration (fixed-length segment-level keywording), and one that uses **automatic shot boundary detection** based on scene-content to get the video segments (semantic segment-level keywording). 

In both cases, the sequences are then analyzed by the keywording model by aggregating different results from frames within each segment. 

You can perform segment-level keywording as follows:

.. code-block:: Java

   ArrayList<VideoSegmentResult> videoresult = MobiusSDK.predictKeywords(MediaMetadataRetriever videoRetriever, 
                                                               double frameRate, 
                                                               int segmentLength, 
                                                               VideoProcessingProgressCallback callback);


* ``videoRetriever`` is the main ``MediaMetadataRetriever`` object. You can build it using the video Uri as follows:
  .. code-block::

     MediaMetadataRetriever videoRetriever = new MediaMetadataRetriever();
     videoRetriever.setDataSource(this, videoUri); //this is the context of the application

* ``frameRate`` is the number of frames per second that are extracted from the video. For example if set to 3, the method will extract a frame every 0.333 seconds.
* ``segmentLength``  is the duration of a segment over which keywording results will be aggregated. If this is not set, segment length will be determined by our automatic video shot detector.
* ``callback`` this callback is useful when processing a video in a dedicated thread/asynctask. It has two functions:
  *1)* ``publishProgress``\ : publish current progress
  *2)* ``publishProgressLength``\ : returns the current length of the progress
  Since there are multiple steps, these two functions are updated 2-3 times during the process.
* The ``VideoSegmentResult`` has the following information:

.. code-block:: Java

  //start of the sequence in secs
  double start = videoresult.getStart(); 
  //end of the sequence in secs
  double end = videoresult.getEnd(); 
  //Score of the shot detection if available (otherwise null)
  Float score = videoresult.getShotScore();
  //keywording result of the sequence
  KeywordingResult keywordingResult = videoresult.getKeywordingResult(); 

  //You can get the final keywords of the sequence by performing thresholding or top-k:
  ArrayList<Keyword> keywords = keywordingResult.getThresholdedResult(); //Using default threshold


Note that for video-sequence keywording, the representative frame is the central frame of the sequence:

.. code-block:: Java

   double timeStampForCentralFrame = (videoresult.getStart() + videoresult.getEnd())/2;

For automatic video-sequence keywording, you can call the same method but without the ``segmentLength`` parameter, this is because the number of frames is determined automatically using shot boundary detection:

.. code-block:: Java

   ArrayList<VideoSegmentResult> videoresult = MobiusSDK.predictKeywords(MediaMetadataRetriever videoRetriever, 
                                                               double frameRate, 
                                                               VideoProcessingProgressCallback callback);
   ArrayList<VideoSegmentResult> videoresult = MobiusSDK.predictKeywords(MediaMetadataRetriever videoRetriever, 
                                                               double frameRate, 
                                                               float shot_bounds_threshold, 
                                                               VideoProcessingProgressCallback callback); 
   //shot_bounds_threshold>0.f is the threshold to separate sequenece (default is 2f). 
   //Larger threshold results in less sequences and smaller threshold leads to more.


Keywording using custom method
=================================

Alternatively, we also provide the following low-level methods if you want to get the sequences by your own custom method:

.. code-block:: Java

   KeywordingResult result = MobiusSDK.getAveragedKeywords(MediaMetadataRetriever videoRetriever, 
                                                Long startTime, 
                                                Long endTime, 
                                                Integer framesCount); 
   //Takes framesCount frames between startTime and endTime (both in microSeconds),
   //averages predictions in feature space then predicts keywords on the result

   KeywordingResult result = MobiusSDK.getPooledKeywords(MediaMetadataRetriever videoRetriever, 
                                               Long timeStamp, 
                                               Long frameRate, 
                                               Integer framesCount); 
   //Takes _framesCount frames starting from timeStamp (in microseconds) and extracted at 'frameRate' frames per second,
   //performs Gaussian smoothing in feature space, and then predicts keywords on the result.

   KeywordingResult result = MobiusSDK.getPooledKeywords(List<Bitmap> frames); 
   //Does the same as above but using a list of frames instead of a videoRetriever object.


Code Snippet for Segment-Level Keywording using Shot-Detector
================================================================
   
Here's a detailed code snippet to perform segment-level keywording using automatic shot detection in an Activity: 

.. code-block:: Java

   //1) Get the video from Uri. You can get it for example from an Intent via getData()
   Uri videoUri = intent.getData();
   MediaMetadataRetriever videoRetriever = new MediaMetadataRetriever();
   videoRetriever.setDataSource(this, videoUri);

   //2) Set up Async task 
       class RunVideoTagging extends AsyncTask<Void, Integer, Void> {

           //Set up callback
           class AsyncTaskVideoProgressCallback implements VideoProcessingProgressCallback {
               private RunVideoTagging task;
               AsyncTaskVideoProgressCallback(RunVideoTagging task) {this.task = task;}
               @Override
               public void publishProgress(int progress) {
                   try {task.publishProgress(progress);}
                   catch (Exception e) {e.printStackTrace();}
               }

               @Override
               public void publishProgressLength(int progresslength){
                  //for example if you're using a progress bar
                  progressBar.setMax(progresslength);
               }
           }

           ArrayList<VideoSegmentResult> videoResult;
           protected Void doInBackground(Void... VoidInput) {
               //Analyze the video each 0.333 second, these frames are used to detection shot boundaries. 
               //Then the boundaries are thresholded with 1.8f to get the sequences, and each sequence is tagged.
               videoResult = MobiusSDK.predictKeywords(videoRetriever, 
                                                       3, 
                                                       1.8f, 
                                                       new AsyncTaskVideoProgressCallback(this));
           }

           //On start
           public void onPreExecute() {
               super.onPreExecute();
               progressBar.setProgress(0);
           }

           //On progress
           protected void onProgressUpdate(Integer... progress) {
               super.onProgressUpdate(progress);
               progressBar.setProgress(progress[0]);
           }

           //When it's finished
           public void onPostExecute(Void result) {
               super.onPostExecute(result);
               //Do something with videoResult. Each element of the list represents a sequence
               videoRetriever.release();
           }

       }

   //3) Run the processing
   new RunVideoTagging().executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR);
   
Release Resources
^^^^^^^^^^^^^^^^^

You can release the SDK resources as follows:

.. code-block:: Java

   MobiusSDK.release();
