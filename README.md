# DeepLearning Computer Settings and test with darknet (With all the files' links, so don't worry about the version compilance)

1. Create a bootable USB stick on macOS

Download ubuntu-16.04.1-desktop-amd64.iso [[Link](https://drive.google.com/drive/folders/11TobALF_VWeBLsCfoVtJhGeRUMW_Oq1m?usp=sharing)]

Create the bootable USB according to the [[Link](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-macos#6)].

Attention: ubuntu-16.04.3-desktop-amd64.iso always cause error!

2. Install Ubuntu16.04.1

Reinstall by reboot and F12, choose UEFI OPTIONS usb boot;

After login to Ubuntu, double click the icon on the desktop to reinstall ubuntu; Restart from the hard disk;

Attention: If choose from the LEGACY OPTIONS, it will cause the problem of unable to login to ubuntu and go to grub!

3.Install Nvidia GPU driver and CUDA 8.0

Download `NVIDIA-Linux-x86_64-384.59.run` and `cuda_8.0.61_375.26_linux.run` [[Link](https://drive.google.com/drive/folders/11TobALF_VWeBLsCfoVtJhGeRUMW_Oq1m?usp=sharing)]

Need to update from 14.4.01 to **14.4.04** to install the driver(else have error of install driver):
```
sudo apt-get update
sudo apt-get upgrade
lsb_release -a
```
Install the driver:
Ctrl+Alt+F1 -> login -> 
```
sudo service lightdm stop 
sudo chmod 777 ./NVIDIA-Linux-x86_64-384.59.run
sudo ./NVIDIA-Linux-x86_64-384.59.run --no-opengl-files 
reboot
nvidia-smi
```
Install cuda 8.0(do not install driver again):
```
chmod 777 cuda_8.0.61_375.26_linux.run
sudo ./cuda_8.0.61_375.26_linux.run --override --no-opengl-lib     
```
**Attention: OpenGL-lib will cause login loop!!!!!!**(Waste my days!!!)

Change the cuda PATH permanently:
```
sudo gedit ~/.bashrc
```
Add:
```
export PATH=/usr/local/cuda-8.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:$LD_LIBRARY_PATH
```
```
source ~/.bashrc
nvcc --version
```
Don’t need to try to run examples of CUDA.

If cannot find -lglut -lGL:
```
sudo apt-get install freeglut3-dev
sudo apt-get install libgl-dev
```
Attention: Should be CUDA8.0, CUDA9.0 is not supported by matlab2017a and will cause some error later.

4. Download and install cuDNN7.0 (cudnn-8.0-linux-x64-v7.1.tgz) on ubuntu: [[Link](https://drive.google.com/drive/folders/11TobALF_VWeBLsCfoVtJhGeRUMW_Oq1m?usp=sharing)]
```
tar -xzvf cudnn-8.0-linux-x64-v7.1.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda-8.0/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda-8.0/lib64
sudo chmod a+r /usr/local/cuda-8.0/include/cudnn.h 
sudo rm -R cuda
```
Check the version of cudnn:
```
cat /usr/local/cuda-8.0/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

5. Install git cmake ccmake etc.
```
sudo apt-get update
sudo apt-get -y install build-essential cmake cmake-curses-gui git libboost-all-dev libgflags-dev libgoogle-glog-dev uuid-dev libboost-filesystem-dev libboost-system-dev libboost-thread-dev ncurses-dev
sudo apt-get -y install libatlas-base-dev libopenblas-base libopenblas-dev liblapack-dev liblapack3
sudo apt-get -y install libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev liblmdb-dev protobuf-compiler unzip libtbb-dev libtbb2 pkg-config gfortran
sudo apt-get -y install python-protobuf python-scipy python-pip python-dev python-numpy libboost-python-dev python-all-dev python-h5py python-matplotlib python-numpy python-pil python-pydot python-skimage python-sklearn 
sudo apt-get install python3-pip 
```
Download and install Eigen [[Link](https://drive.google.com/drive/folders/11TobALF_VWeBLsCfoVtJhGeRUMW_Oq1m?usp=sharing)]:
```
tar xzvf eigen-3.3.4.tar.gz 
cd eigen
mkdir build
cd build
cmake ..
sudo make install
```

6. Download and install OpenCV [[Link](https://drive.google.com/drive/folders/11TobALF_VWeBLsCfoVtJhGeRUMW_Oq1m?usp=sharing)]:
```
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

cd opencv
mkdir release
cd release
ccmake ..
```
**set `OPENCV_EXTRA_MODULE_PATH` as `/media/elab/sdd/Amy/opencv_contrib/modules`**,  configure, configure and generate, then:
```
cmake -D OPENCV_EXTRA_MODULE_PATH=/media/elab/sdd/Amy/opencv_contrib/modules -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
make -j`nproc` 
sudo make install
```
Add the path permanently:
```
sudo gedit ~/.bashrc
```
Add:
```
export PATH=/usr/local/cuda-8.0/bin:/usr/local/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:/usr/local/lib:$LD_LIBRARY_PATH
```
and don't forget:
```
source ~/.bashrc
```

7. Install Darknet
```
git clone https://github.com/rockkingjy/darknet
```
Create folder weights/ in the darknet/, download the weights [[Link](https://drive.google.com/drive/folders/1DD1qv4fm-bcdeQIYoB1t_-XciVXq4xxr?usp=sharing)], put all the weights inside, set the Makefile:
```
GPU=1
CUDNN=1
OPENMP=1
```
then run:
```
./darknet detector demo cfg/coco.data cfg/yolov3.cfg weights/yolov3.weights
```
If it run successfully, you will see the predictions.png in darknet/, that also mean your CUDA and cudnn installs correctly.

8. Install Caffe (Direct makefile install and more details in https://github.com/rockkingjy/DeepLearningSettings/blob/master/caffe.md).

Update CMake to compatible with Boost version:
```
Boost 1.63 requires CMake 3.7 or newer.
Boost 1.64 requires CMake 3.8 or newer.
Boost 1.65 and 1.65.1 require CMake 3.9.3 or newer.
Boost 1.66 requires CMake 3.11 or newer.
Boost 1.67 is only supported by CMake master since March 2018.
```
```
apt-get remove cmake
git clone https://gitlab.kitware.com/cmake/cmake.git
cd cmake
./bootstrap
make -j12
make install
```
Download boost [[Link](https://drive.google.com/drive/folders/11TobALF_VWeBLsCfoVtJhGeRUMW_Oq1m?usp=sharing)] and install:
```
sudo ./bootstrap.sh
sudo ./b2 install
```
It will install in `\usr\local\include\boost` and `\usr\local\lib` automatically.

If you use python 2.7, change the file in `caffe\cmake\Dependencies.cmake', replace:
```
    find_package(Boost 1.46 COMPONENTS python)
```
with 
```
    find_package(Boost 1.46 COMPONENTS python27)
    set(Boost_PYTHON_FOUND ${Boost_python27}_FOUND})
```
Then camke, make and install:
```
git clone https://github.com/rockkingjy/caffe
cd caffe
mkdir build
cmake ..
make clean
make all -j`nproc`
make pycaffe
make install
```
then follow https://github.com/rockkingjy/Inference_RGB2D_caffe to run a RGB2Depth programme.

**Attention!!! If caffe/build/ exists, remember first to delete it! If cmake .. directly, will create error!!!**

9. Install matlab2017 following: https://github.com/rockkingjy/DeepLearningSettings/blob/master/matlab.md

