# ROS Wrapper around DSO: Direct Sparse Odometry. (catkin version)

For more information see https://vision.in.tum.de/dso

This is meant as simple, minimal example of how to integrate DSO from a different project, and run it on real-time input data. It does not provide a full ROS interface (no reconfigure / pointcloud output / pose output). To access computed information in real-time, I recommend to implement your own Output3DWrapper; see the DSO code.

### 

### Related Papers

- **Direct Sparse Odometry**, *J. Engel, V. Koltun, D. Cremers*, In arXiv:1607.02565, 2016
- **A Photometrically Calibrated Benchmark For Monocular Visual Odometry**, *J. Engel, V. Usenko, D. Cremers*, In arXiv:1607.02555, 2016

# 

# 1. Installation

1. Install DSO. We need DSO to be compiled with OpenCV (to read the vignette image), and with Pangolin (for 3D visualization).

2. Create your workspace. 

   ```
   mkdir -p  ~/catkin_ws/src
   cd  ~/catkin_ws
   catkin_make
   ```

3. Install and compile dso_ros.

   ```shell
    cd  ~/catkin_ws/src
    git clone https://github.com/CLM666/dso_ros.git
    cd ..
    catkin_make
   ```

   Note: make sure you have set available DSO_PATH in CMakeLists.txt.

# 2 Usage

For real-time camera input, use these commands in two terminals. 

```
terminal 1:
roscore
	
terminal 2:
cd  ~/catkin_ws
source devel/setup.bash
rosrun dso_ros dso_ros image:=image_raw \
	calib=XXXXX/camera.txt \
	gamma=XXXXX/pcalib.txt \
	vignette=XXXXX/vignette.png \
```

For EUROC dataset (example MH_05), use these commands in three terminals.

```
terminal 1:
roscore

terminal 2:
cd  ~/catkin_ws
source devel/setup.bash
rosrun dso_ros dso_ros calib=./src/dso_ros/calibration/euroc.txt  image:=/cam0/image_raw preset=0 mode=1

terminal 3:
cd  ~/catkin_ws
source devel/setup.bash
rosbag play --pause  XXXXX/MH_05_difficult.bag
```

Note: "--pause" means you can press space key to control the process of bag playing. "XXXXX"means the absolute path to your folder.  

## 3 Accessing Data.

See the DSO Readme. As of now, there is no default ROS-based `Output3DWrapper` - you will have to write your own.

# 

# 4 Dependencies

## 

## 4.1 Pangolin

removing

```
    fullSystem->outputWrapper = new IOWrap::PangolinDSOViewer(
    		 (int)undistorter->getSize()[0],
    		 (int)undistorter->getSize()[1]);
```

will allow you to use DSO compiled without Pangolin. However, then there is no 3D visualization. You can also implement your own Output3DWrapper to fit your needs.

## 

## 4.2 OpenCV

you can use DSO compiled without OpenCV. In that case, the vignette image will not be read, and no photometric  calibration can be used. Also, there will not be any image  visualizations / image saving. You can also implement your own version of ImageRW.h / ImageDisplay.h,  instead of the dummies.

### 

### 5 License

This ROS wrapper around DSO is licensed under the GNU General Public License Version 3 (GPLv3). For commercial purposes, we also offer a professional version, see http://vision.in.tum.de/dso for details.
