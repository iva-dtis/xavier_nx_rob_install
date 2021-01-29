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
	-D OPENCV_EXTRA_MODULES_PATH=/data/install/opencv_build/opencv_contrib/modules \
        -D PYTHON_EXECUTABLE=/usr/bin/python3 \
	-D BUILD_EXAMPLES=ON /data/install/opencv_build/opencv

make -j4
sudo make install

```

# build ROS1 from source on Python3

```
sudo apt install  python3 python3-dev python3-pip build-essential libffi-dev
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

sudo pip3 install cython numpy --upgrade
sudo pip3 install matplotlib pyqtgraph ipython[all] --upgrade
sudo pip3 install scipy scikit-image sckit-learn sympy --upgrade
sudo pip3 install -U -f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-18.04 wxPython

```


```
export ROS_PYTHON_VERSION=3

sudo apt install libtinyxml-dev google-mock libgtest-dev libpcl-dev sbcl libboost-thread-dev libboost-date-time-dev libboost-system-dev liborocos-kdl-dev libbz2-dev libboost-filesystem-dev libconsole-bridge-dev libgpgme-dev libssl-dev liblz4-dev libboost-all-dev libboost-program-options-dev libboost-regex-dev liburdfdom-headers-dev libtinyxml2-dev pyqt5-dev python3-pyqt5 python3-pyqt5.qtsvg python3-sip-dev python3-netifaces libboost-dev python3-pyqt5.qtwebkit libcurl4-openssl-dev curl python3-pydot python3-pygraphviz hddtemp python3-psutil libyaml-cpp-dev python3-opengl libboost-chrono-dev libogre-1.9-dev liburdfdom-dev libassimp-dev python3-defusedxml graphviz libgtk2.0-dev libfltk1.3-dev libjpeg-dev libtool-bin libpoco-dev python3-pyqt5.qtopengl libapr1-dev libaprutil1-dev liblog4cxx-dev python3-mock python3-empy python3-pil python3-pycryptodome python3-gnupg python3-coverage python3-nose  gazebo9 libgazebo9-dev

```
in /src/vision_opencv/cv_bridge/CMakeLists.txt modify 
```
    find_package(Boost REQUIRED python37)
```
to
```
    find_package(Boost REQUIRED python3)
```

```
catkin build

```


# installlibrealsense


sudo apt-key adv --keyserver keys.gnupg.net --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE

sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo bionic main" -u

sudo apt-get install librealsense2-dev librealsense2-utils cmake-curses-gui

# install nodejs

```
cd /data/install
curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh
chmod +x nodesource_setup.sh 
sudo ./nodesource_setup.sh 
sudo apt-get install  nodejs 
```

```
git clone https://github.com/RobotWebTools/roslibjs
cd roslibjs 
npm install
```


# build ROS2 from source


```
sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8


sudo apt update && sudo apt install curl gnupg2 lsb-release
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'

sudo apt update && sudo apt install  \
  build-essential \
  cmake \
  git \
  libbullet-dev \
  python3-colcon-common-extensions \
  python3-flake8 \
  python3-pip \
  python3-pytest-cov \
  python3-rosdep \
  python3-setuptools \
  python3-vcstool \
  wget

python3 -m pip install -U \
  argcomplete \
  flake8-blind-except \
  flake8-builtins \
  flake8-class-newline \
  flake8-comprehensions \
  flake8-deprecated \
  flake8-docstrings \
  flake8-import-order \
  flake8-quotes \
  pytest-repeat \
  pytest-rerunfailures \
  pytest


sudo apt install --no-install-recommends  \
  libasio-dev \
  libtinyxml2-dev

sudo apt install --no-install-recommends  \
  libcunit1-dev


sudo apt install clang
```


```
mkdir -p /data/ros/ros2_foxy/src
cd /data/ros/ros2_foxy
#https://raw.githubusercontent.com/ros2/ros2/foxy-release/ros2.repos
wget https://raw.githubusercontent.com/ros2/ros2/foxy/ros2.repos
vcs import src < ros2.repos
```


```
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro foxy -y --skip-keys "console_bridge fastcdr fastrtps rti-connext-dds-5.3.1 urdfdom_headers opencv "
sudo apt purge libopencv*

```




```
colcon build --symlink-install
```




# Mapping

```
sudo apt-get install -y python3-wstool python3-rosdep ninja-build stow
sudo apt install libceres-dev


```
```
cd /data/install/
curl -R -O http://www.lua.org/ftp/lua-5.3.4.tar.gz
tar -zxf lua-5.3.4.tar.gz
cd lua-5.3.4
make linux test
sudo make install
sudo apt install libqglviewer2-qt5 libqglviewer-dev-qt5
sudo apt install libsdl2-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-gfx-dev libsdl2-net-dev libsdl2-ttf-dev libglew-dev freeglut3-dev glew-utils
sudo apt install libsdl1.2-dev libsdl-pango-dev libsdl-image1.2-dev
sudo apt install libflann-dev libann-dev
sudo apt install libpng++-dev libpng-dev
sudo apt install libusb-dev 

```







#3 pytorch

wget https://nvidia.box.com/shared/static/yr6sjswn25z7oankw8zy1roow9cy5ur1.whl -O torch-1.6.0rc2-cp36-cp36m-linux_aarch64.whl
sudo apt-get install  libopenblas-base lbopenblas-dev ibopenmpi-dev
sudo pip3 install torch-1.6.0rc2-cp36-cp36m-linux_aarch64.whl

sudo apt-get install libjpeg-dev zlib1g-dev
git clone --branch v0.6.0 https://github.com/pytorch/vision torchvision
cd torchvision
sudo python3 setup.py install



# trello



sudo add-apt-repository ppa:jonathonf/ffmpeg-4
sudo apt upgrade
sudo pip3 install av

sudo apt install python3-openssl 
sudo pip3 install twisted



# build librealsense

sudo apt purge librealsense2*

sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev -y
sudo apt-get install libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev at 
cd /data/install
git clone https://github.com/IntelRealSense/librealsense
cd librealsense
./scripts/patch-realsense-ubuntu-L4T.sh  

./scripts/setup_udev_rules.sh  
mkdir build && cd build  
cmake .. -DBUILD_EXAMPLES=true -DCMAKE_BUILD_TYPE=release -DFORCE_RSUSB_BACKEND=false -DBUILD_WITH_CUDA=true && make -j$(($(nproc)-1)) && sudo make install



