# DeepLearningSettings

1. Create a bootable USB stick on macOS

Download ubuntu-16.04.1-desktop-amd64.iso [[Link](https://drive.google.com/drive/folders/11TobALF_VWeBLsCfoVtJhGeRUMW_Oq1m?usp=sharing)]

Create the bootable USB according to the [[Link](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-macos#6)].

Attention: ubuntu-16.04.3-desktop-amd64.iso always cause error!

2. Install Ubuntu16.04.1

Reinstall by reboot and F12, choose UEFI OPTIONS usb boot;

After login to Ubuntu, double click the icon on the desktop to reinstall ubuntu; Restart from the hard disk;

Attention: If choose from the LEGACY OPTIONS, it will cause the problem of unable to login to ubuntu and go to grub!

3.Install Nvidia GPU driver and CUDA 8.0

Download cuda_8.0.61_375.26_linux.run [[Link](https://drive.google.com/drive/folders/11TobALF_VWeBLsCfoVtJhGeRUMW_Oq1m?usp=sharing)]

Ctrl+Alt+F1 -> login -> 
```
sudo service lightdm stop 
chmod 777 cuda_8.0.61_375.26_linux.run
sudo ./cuda_8.0.61_375.26_linux.run --override --no-opengl-lib     
reboot
nvidia-smi
```
Attention: OpenGL-lib will cause login loop!!!!!!(Waste my days!!!)

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
nvcc —-version
```
Don’t need to try to run examples of CUDA.

If cannot find -lglut -lGL:
```
sudo apt-get install freeglut3-dev
sudo apt-get install libgl-dev
```

Attention: Should be CUDA8.0, CUDA9.0 will cause some error later.

4. Download and install cuDNN7.0 (cudnn-8.0-linux-x64-v7.1.tgz) on ubuntu: [[Link](https://drive.google.com/drive/folders/11TobALF_VWeBLsCfoVtJhGeRUMW_Oq1m?usp=sharing)]
```
tar -xzvf cudnn-8.0-linux-x64-v7.1.tgz
sudo cp cuda/include/cudnn.h /home/elab/yanInstall/cuda-8.0/include
sudo cp cuda/lib64/libcudnn* /home/elab/yanInstall/cuda-8.0/lib64
sudo chmod a+r /home/elab/yanInstall/cuda-8.0/include/cudnn.h 
```
Check the version of cudnn:
```
cat /home/elab/yanInstall/cuda-8.0/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

5. Install git
```
sudo apt-get install git
```

6. Install Darknet and download the weights [[Link](https://drive.google.com/drive/folders/1DD1qv4fm-bcdeQIYoB1t_-XciVXq4xxr?usp=sharing)]
```
git clone https://github.com/rockkingjy/darknet
```
Create folder weights/ in the darknet/, put all the weights inside, set the Makefile+
```
GPU=1
CUDNN=1
OPENMP=1
```
then run:
```
./darknet detector demo cfg/coco.data cfg/yolov2.cfg weights/yolo.weights 
```
If it run successfully, you will see the predictions.png in darknet/, that also mean your CUDA and cudnn installs corectly.

