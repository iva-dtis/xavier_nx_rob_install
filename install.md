# Post installation 

```bash
sudo apt update
sudo apt install gparted geeqie terminator git screen openssh-server tree htop 
sudo apt purge libreoffice*
sudo apt purge thunderbird
sudo apt purge *opencv*
sudo apt autoremove
sudo apt upgrade
```

# Setup NVme disk

```
sudo mkdir /data
```


On gparted :

- create partition table on Nvme

- create 16Go of swap on nvme

- create a ext4 with all remaned space 

Get UUID on the created ext4 partition and add this line to /etc/fstab:

```
UUID=[theuuid]      /data   ext4    defaults        0       0 
```

idem get uuid of swap partition and add this lline to /etc/fstab :
```
UUID=bb269471-a627-461c-a2be-b760d84999f6	none	swap	sw	0	0
```

```
sudo mount /data
sudo chown $USER /data/
```


# Install OpenCV

```
sudo apt install build-essential cmake git pkg-config libgtk-3-dev \
    libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
    libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
    gfortran openexr libatlas-base-dev python3-dev python3-numpy \
    libtbb2 libtbb-dev libdc1394-22-dev  liblapack-dev  x265 libx265-dev \
    libx264-dev tesseract-ocr tesseract-ocr-eng tesseract-ocr-fra  libtesseract-dev \
    libjpeg8-dev libpng-dev libgtk-3-dev libtbb-dev qt5-default  libavcodec-dev \
    libavformat-dev libswscale-dev libv4l-dev libdc1394-22-dev libgstreamer1.0-dev \
    libgstreamer-plugins-base1.0-dev libatlas-base-dev libfaac-dev libmp3lame-dev \
    libtheora-dev libxvidcore-dev libx264-dev libopencore-amrnb-dev libopencore-amrwb-dev \
    libgphoto2-dev libeigen3-dev libhdf5-dev doxygen x264 v4l-utils libgoogle-glog-dev 
```

```
mkdir /data/opencv_build && cd /data/opencv_build
git clone https://github.com/opencv/opencv.git
git clone https://github.com/opencv/opencv_contrib.git
```

```
mkdir -p /data/opencv_build/build
cd /data/opencv_build/build

cmake -D CMAKE_BUILD_TYPE=RELEASE \
	-D CMAKE_INSTALL_PREFIX=/usr/local \
	-D INSTALL_C_EXAMPLES=ON \
	-D INSTALL_PYTHON_EXAMPLES=ON \
	-D OPENCV_EXTRA_MODULES_PATH=/data/opencv_build/opencv_contrib/modules \
        -D PYTHON_EXECUTABLE=/usr/bin/python3 \
	-D BUILD_EXAMPLES=ON /data/opencv_build/opencv

make -j4
sudo make install

```

# build ROS1 from source on Python3

```
sudo apt install  python3 python3-dev python3-pip build-essential
sudo -H pip3 install rosdep rospkg rosinstall_generator rosinstall wstool vcstools catkin_tools catkin_pkg
sudo rosdep init
rosdep update
```


```
mkdir -p /data/ros/ros_catkin_ws/src
cd /data/ros/ros_catkin_ws
catkin config --init -DCMAKE_BUILD_TYPE=Release --blacklist rqt_rviz rviz_plugin_tutorials librviz_tutorial --install

rosinstall_generator desktop_full --rosdistro noetic --deps --tar >noetic-desktop-full.rosinstall
wstool init -j8 src noetic-desktop-full.rosinstall
```






# build ROS2 from source




























