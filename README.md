# Enable Unitree Go1 Cameras

This package allows you to enable the Unitree Go1 cameras so that they can be streamed by standard Linux video tools. Upon powering on, the cameras to the Unitree Go1 are locked. When their SDK is launched, it passes some arguments to enable the cameras, and upon shutdown, it disables them again. 

I did not want to use their CameraSDK to create a video pipeline, so I discovered that if you start their SDK to initialize a camera, then `abort()`, the camera stays unlocked and you can stream from it using commands like `ffmpeg` or ROS `usb_cam`. 

In order for their SDK to compile, you need to install opencv 4.1.1 locally. Then when you build the project, you need to point to the location of this local installation. The file `local_opencv.tar` contains the folder structure that you need to compile it. 

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
