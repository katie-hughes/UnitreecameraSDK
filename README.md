# Enable Unitree Go1 Cameras

This package allows you to enable the Unitree Go1 cameras so that they can be streamed by standard Linux video tools. 
Upon powering on, the cameras to the Unitree Go1 are locked. 
If you try to access them using native tools like `ffmpeg` you will get errors about codec not found and no data being encoded.
When their SDK is launched, it passes some arguments to enable the cameras, and upon shutdown, it disables them again.
This essentially locks you into using their SDK if you want to do anything involving the onboard cameras.


I did not want to use their CameraSDK to create a video pipeline, especially after the discovery of spyware on the Raspberry Pi that streams MQTT data (including camera data) to a VPN in China.  
I discovered that if you start their SDK to initialize a camera, then `abort()`, the camera stays unlocked and you can stream from it using commands like `ffmpeg` or ROS `usb_cam`. 


To set up:
```
git clone git@github.com:katie-hughes/UnitreecameraSDK.git
cd UnitreeCameraSDK
mkdir build
cd build
cmake ..
make
cd ../examples/bins
./unlock_camera0
./unlock_camera1
```

## More Advanced Usage
the UnitreeCameraSDK has an unpleasant dependancy on OpenCV4.1.1. If you care about being able to lock the cameras again, or want to use any of their functionality, you will need to install this version of OpenCV locally if you do not already have it. The file `local_opencv.tar` contains the folder structure that you need for compilation. If you want this, build the project with the following CMake arguments:
```
cmake -DUNITREE_CAMERA_USE_CUSTOM_OPENCV_DIR=TRUE -DUNITREE_CAMERA_OPENCV_DIR=${opencv_4_1_1_dir} ..

```
Then you will be able to lock the cameras again with:
```
./lock_camera0
./lock_camera1
```
