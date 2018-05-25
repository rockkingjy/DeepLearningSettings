## Install
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
