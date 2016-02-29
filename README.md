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
  ...
```

*  Check the OpenCV2 python bindings exist:
```
 >>> import cv2

```
* Compile the package. Move the code directory into your `catkin_ws/src` and call `catkin_make` 

    *  relevant ros packages: roscpp, rospy, std_msgs, image_transport, cv_bridge, sensor_msgs, geometry_msgs


### Overview of files in Obj_Detector Repository:

* package.xml
* CMakeLists.txt
* launch/
    * cone_detector.launch
    * ...
* src/
    * blob.png
    * image.png
    * coney.py
    * edge.py
    * echo.py
    * echo.cpp
    * MatchTemplate_Demo.cpp

#### CMakeLists

`CMakeLists.txt` finds and links the OpenCV libraries and ros packages.  If you create more .cpp files you need to declare a c++ executable and add dependencies to it. 

Add the bottom of CMakeLists.txt we added the following to make `template_node`:
```
add_dependencies(template_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(template_node ${OpenCV_LIBS} ${catkin_LIBRARIES}  )
#)

```

#### Launch/cone_detector.launch
This package creates ros subscribers that subscribe to `~image_raw` to obtain images from a camera feed.  The launch file shows how to remap `~image_raw` to `/camera/rgb/image_rect_color`.  Modify this file to subscribe to a different image feed.


### Source Files Explanations

#### echo.py and echo.cpp  (CV_BRIDGE Examples)
Both echo programs subscribe to our image topic and publish the image they recieved.  Before publishing the image message they convert the message to opencv type using cv_bridge.

In python:
```python
#convert sensor_msgs.msg/Image.msg  to opencv
self.bridge.imgmsg_to_cv2(image_msg)

#convert opencv image to sensor_msgs.msg/Image.msg:
self.bridge.cv2_to_imgmsg(image_cv, "bgr8")
```

In cpp:
```
// convert sensor_msgs.msg/Image.msg to opencv Mat
image = cv_bridge::toCvCopy(msg, "bgr8")->image;

// convert Mat to sensor_mgs.msg/Image.msg
 msg = cv_bridge::CvImage(
            std_msgs::Header(), "bgr8", image).toImageMsg();

```

#### image_matching.py (Template Matching Example)
This program uses a template image and tries to find it inside the camera feed.
To run this program, you must include the template image location:

```
roslaunch cone_detector cone_detector.launch image:=/home/ari/catkin_ws/src/cone/cone_detector/src/blob.png 
```

This program defines two classes: `ConeDetector` and TemplateMatcher`.

* ConeDetector should look very similiar to Echo.py. It has an additional ros publisher for the `cone_ibvs` topic.  This publishes a signal from [-1,1] of the bcones relative location and was used for the TA's solution of IBVS parking at a cone.

* TemplateMatcher uses OpenCV's `matchTemplate()` function to compare the template image to the raw image frame.   You can read about it more on their website here: http://docs.opencv.org/master/d4/dc6/tutorial_py_template_matching.html#gsc.tab=0

As you'll see many vision algorithms have different qualities.  Template matching is very susceptible to scale and rotation error.  An upright orange cone is symmetrical and will not have rotation error, however the scale will change as the robot moves toward and away the cone. We have implemented pyramid matching functionality that resizes the template image for comparison.



