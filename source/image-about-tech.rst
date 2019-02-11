About Mobius Vision Image SDK
======================================

This SDK uses advanced algorithms for computer vision based on a technology called Deep Learning (DL).

DL technology has helped computers to understand images not just as a matrix of pixels
but to connect different levels such as lines, simple geometric shapes to complex objects such as faces and cars.
It gives computers some sense of hierarchical understanding on the content of images.

With this hierarchical understanding, computers can recognize complex objects in images.

The SDK is shipped with two base models and a customizable mini-model that is built on top of the base models.
The reasoning behind this set up is that customization needs far less effort as the base models already include
lots of knowledge which can be leveraged directly for new objects or styles.

Additionally, this SDK is specifically for mobile phones.

.. improve text on mobile stuff

Object recognition with keywords
------------------------------------

The Mobius mobile SDK comes with a model for keywords that the computer can see in a given image.
Our over 6000 keywords have different levels of granularity. It differs between people and no people but also on a lower level
between men and women.
Given one image as an input, the model returns the keywords for the objects it detected with highest confidence.

.. image::
   data/keywords_tree.png
   :height: 400 px
   :width: 600 px
   :align: center

Currently, the SDK does not enable to pinpoint to the objects. So the computer recognizes that
there is an object in the given image but not where.

Aesthetics evaluation
-----------------------

The second model that is included in the Mobius mobile SDK is a model to evaluate the aesthetics of an image.

.. improve explanation here

Customizable mini-model
------------------------

One powerful tool that comes with the SDK is a customizable and reusable mini-model.
It is built on top of our object recognition and aesthetics base models - so it can use their knowledge to understand properties of new objects or styles.

Mobile models
---------------

The models in this SDK are specifically adapted to run on edge (on the device).
This has great advantages such as:

* Data privacy and security by design: the data always stays on the phone
* Offline processing possible: no need for access to the internet
* Analysis of images on the camera roll of the user: possible to recommend which image to upload to an app

at a moderate cost of:

* Uses computational resources of the phone (specifically for training that might take a few seconds)
* Increased battery usage
* Increased requirements for installation
