Video Highlighting
------------------

The SDK provides a tool to analyze videos and extract highlights. The main call is the following: 

.. code-block:: Java

   HighlightingResult highlightingResult = MobiusSDK.getHighlightingResult(MediaMetadataRetriever videoRetriever,double frameRate, VideoProcessingProgressCallback callback);

The parameters of the call above are detailed in the following:


* ``videoRetriever`` is the main ``MediaMetadataRetriever`` object. You can build it using the video Uri as follows:
  .. code-block::

     MediaMetadataRetriever videoRetriever = new MediaMetadataRetriever();
     videoRetriever.setDataSource(this, videoUri); //this is the context of the application

* ``frameRate`` is the number of frames per second that are extracted from the video. For example if set to 3, the method will extract a frame every 0.333 seconds.
* ``callback`` this callback is useful when processing a video in a dedicated thread/asynctask. It has two functions:
  *1)* ``publishProgress``\ : publish current progress
  *2)* ``publishProgressLength``\ : returns the current length of the progress

From the ``HighlightingResult`` object you can get the highlighting score per timestamp:

.. code-block:: Java

   TreeMap<Long,Float> highlightingScores = highlightingResult.getScores();
   //The key is the timestamp and the value is the score.

The highlighting score reflects the likelihood that a given frame at the timestamp is part of a highlight. The higher the score the more likely it is that the frame is part of the highlight.

Code Snippet for Video Highlighting
================================================================

Here is a detailed code snippet to use in an Activity: 

.. code-block:: Java

   //1) Get the video from Uri. You can get it for example from an Intent via getData()
   Uri videoUri = intent.getData();
   MediaMetadataRetriever videoRetriever = new MediaMetadataRetriever();
   videoRetriever.setDataSource(this, videoUri);
   
   //2) Set up Async task 
   class RunVideoHighlightingTask extends AsyncTask<Void, Integer, Void> {
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
           //Main video highlighting result
           HighlightingResult highlightingResult = null; 
           protected Void doInBackground(Void... VoidInput) {
               //Analyze the video every 0.333 seconds, these frames are used to  calculate the highlighting scores.
               highlightingResult = MobiusSDK.getHighlightingResult(videoRetriever, 
                                                       3, //Frame Rate -> 3 means we extract a frame every 0.333 seconds
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
               //You can update the progress bar here
               progressBar.setProgress(progress[0]);
           }
           //When it's finished
           public void onPostExecute(Void result) {
               super.onPostExecute(result);
               //Get the highlighting score for each timeStamp
               TreeMap<Long,Float> highlightingScores = highlightingResult.getScores();
               //Call to UI goes here 
               //Release video retriever object
               videoRetriever.release();
           }
       }
       
   //3) Run task 
   new RunVideoTagging().executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR);

Release Resources
^^^^^^^^^^^^^^^^^^

You can release the SDK resources as follows:

.. code-block:: Java

   MobiusSDK.release();