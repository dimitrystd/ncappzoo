# Customizations
* Tried to improve quality of face matching. Added HOG for `video_face_matcher_multipleFace` app. Works better than the original app but really slow
  * Standard app returns `min_distance` 0.7-1
  * New app returns `min_distance` 0.08-0.3. But 950-1100msec takes HOG
  * Used [this article](https://www.pyimagesearch.com/2018/04/02/faster-facial-landmark-detector-with-dlib/) as example
* Quick test results (executed on `raspberry 3 B+`)
    ```
    Use HOG
    [INFO] elasped time: 32.61
    [INFO] approx. FPS: 0.74
    
    Use HAAR
    [INFO] elasped time: 21.95
    [INFO] approx. FPS: 1.23
    
    Use https://www.pyimagesearch.com/2018/06/25/raspberry-pi-face-recognition/
    [INFO] elasped time: 20.02
    [INFO] approx. FPS: 2.05
    ```
    The last one doesn't use NCS but works faster, but of course less accuracy

# Introduction
The video_face_matcher_multipleFace example app uses the TensorFlow [ncappzoo/tensorflow/facenet](../../tensorflow/facenet) neural network to find a face in a video camera stream that matches with a list of known face image.  

The provided video_face_matcher_multipleFace.py python program starts a webcam and shows the camera preview in a GUI window.  When the camera preview shows a face that matches one of the known valid faces saved in (video_face_matcher_multipleFace/validated_images/) a green frame is displayed around the window to indicate a match was found.

# Details
To make the program recognize your face or any face that you would like add the image file to video_face_matcher_multipleFace/validated_images/ to recognize.

To determine a match the FACE_MATCH_THRESHOLD value is used.  You might want to adjust this value keeping in mind that the closer this value is to 0.0, the more exact and less forgiving the matching will be.  The initial value for FACE_MATCH_THRESHOLD is 0.8 but you will likely want to play with this value for your own needs.

The provided Makefile does the following:
1. Makes the facenet graph file from the ncappzoo/tensorflow/facenet example and copies it to the base directory of this project.
2. Runs the provided video_face_matcher_multipleFace.py program that creates a GUI window which shows the camera stream and match/not match status.

# Prerequisites
This program requires:
- 1 NCS device
- A webcam
- NCSDK 1.12.00 or greater
- The ncappzoo/tensorflow/facenet make compile command must work.  See [the facenet README.md](../../tensorflow/facenet/README.md) for details
- opencv 3.3 with video for linux support

Note: The OpenCV version that installs with the some versions of ncsdk does <strong>not</strong> provide V4L support.  To run this application you will need to replace the ncsdk version with a version built from source.  To remove the old opencv and build and install a compatible version you can run the following command from the app's base directory:

```
   make opencv
```   
Note: All development and testing has been done on Ubuntu 16.04 on an x86-64 machine.

# Makefile
Provided Makefile has various targets that help with the above mentioned tasks.

## make help
Shows available targets.

## make all
Builds and/or gathers all the required files needed to run the example python program. 

## make compile
runs make compile in the ncappzoo/tensorflow/facenet project to create the facenet graph file (facenet_celeb_ncs.graph) for the NCS device, and copies it to the base directory.

## make run_py
Runs the provided python program demonstrating the facenet network.

## make clean
Removes all the temporary files that are created by the Makefile.  Also removes 20170512-110547.zip.

