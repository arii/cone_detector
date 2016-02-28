# obj_detector
ROS + OpenCV 2.4.8 integration.  Examples of object detection using  template matching, countour finding, and morphology with rospy and roscpp


### Examples
Video of using stuff


### Verify Installation Requirements 
To use this software you need ros-indigo, OpenCV 2.4.8, and python. 

* Verify OpenCV release files exist for 2.4.8:
```
cat /usr/share/OpenCV/OpenCVModules-release.cmake | grep 2.4.8
```
You should see a lot of printouts like this:
```
  IMPORTED_LOCATION_RELEASE "${_IMPORT_PREFIX}/lib/x86_64-linux-gnu/libopencv_core.so.2.4.8"
```

*  Check the OpenCV2 python bindings exist:
```
 >>> import cv2

```
* Compile the package. Move the code directory into your `catkin_ws/src` and call `catkin_make` 


### Getting Started: OpenCV Bridge


