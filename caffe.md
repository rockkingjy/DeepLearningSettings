## Careful! Before compile, you need to change back to set high priority to gcc-5 to compile caffe if you've used gcc-4.9 before.
```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 50
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 100
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.9 50
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 100
```
## Install by makefile on Linux
```
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
```
Download and compile nccl:
```
cd nccl
make -j`nproc`
sudo make install
```
Change caffe/Makefile.config:
```
USE_CUDNN := 1
USE_NCCL := 1
OPENCV_VERSION := 3
WITH_PYTHON_LAYER := 1

INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include  /usr/include/hdf5/serial 
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu 
```
Change caffe/Makefile to prevent opencv link error:
```
ifeq ($(USE_OPENCV), 1)
	LIBRARIES += opencv_core opencv_highgui opencv_imgproc 

	ifeq ($(OPENCV_VERSION), 3)
		LIBRARIES += opencv_imgcodecs
		LIBRARIES += opencv_videoio
	endif
		
endif
```
Then in caffe/:
```
make clean
make -j`nproc`
```
And add the lib to the ~/.bashrc for future use:
```
export LD_LIBRARY_PATH=/media/elab/sdd/caffe/build/lib:$LD_LIBRARY_PATH
```
====================================================
## Install on Mac
```
brew install boost
brew install protobuf
brew install hdf5
brew install lmdb
brew install leveldb
brew install glog
```
```
mkdir build
cd build
cmake ..
make -j`nproc`
make install
```
====================================================
## Draw the nets
```
python ./python/draw_net.py train.prototxt pic.png
```

## Save log to a file:
```
python train.py 2>&1 | tee train.log
```
if not saved, try to find it in **/tmp/caffe.***.***.log**

## Parse log:
copy the train.log to caffe/tools/extra
```
.tools/extra/parse_log.sh <train.log>
```

## Plot log:
```
.tools/extra/plot_training_log.py 0 <save.png> <train.log>
```
