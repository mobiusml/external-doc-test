Advanced: Visual Image Search
===============================


Search for similar images using query keywords
---------------------------------------------------

.. note::

  Features have to be extracted and cached in order to do search

Search for images tagged with query keyword 'sunset'.
::

  String[] searchKeywords = new String[]{"sunset"};
  ArrayList<Pair<?,Float>> searchResult = MobiusSDK.getSortedSearchResults(searchKeywords, cachedFeatures);

Search for images tagged with **all** the query keywords provided in the list 'sunset','sky','beach'.

::

  String[] searchKeywords = new String[]{"sunset","sky","beach"};
  ArrayList<Pair<?,Float>> searchResult = MobiusSDK.getSortedSearchResults(searchKeywords, cachedFeatures);

Let's say we get lots of results of images including sunset, sky and beach but some of them
also show architecture but we'd like to have a photo without any buildings.
We can also subtract keywords - things that should not appear in our results.
For that we need to define a separate list of keywords that can be passed to the search function.

::

  String[] searchKeywordsPos = new String[]{"sunset","sky","beach"};
  String[] searchKeywordsNeg = new String[]{"architecture"}
  ArrayList<Pair<?,Float>> searchResult = MobiusSDK.getSortedSearchResults(searchKeywordsPos, searchKeywordsNeg, cachedFeatures);



Search by visual similarity using a query image
-------------------------------------------------

Search is performed by passing an image (Bitmap) or its features (:java:`float[]`) and a set of cached features of other images in Map form as follows:

::

  int flag = MobiusSDK.SEARCH_KEYWORDING_ONLY; //Search only based on keywording information
  int flag = MobiusSDK.SEARCH_KEYWORDING_AND_AESTHETICS; //Search only based on both keywording and aesthetics
  ArrayList<Pair<?,Float>> result = MobiusSDK.getSortedSearchResults(Bitmap bitmap, Map<?,float[]> cachedFeatures, flag)
  //cachedFeatures here can be Map<Uri,float[]> for instance, which will return ArrayList<Pair<Uri,Float>>

::

  //search from float[] features instead of a bitmap
  ArrayList<Pair<?,Float>> result = MobiusSDK.getSortedSearchResults(float[] features, Map<?,float[]> cachedFeatures, flag)
