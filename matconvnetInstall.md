Install Matconvenet:

## Careful! Need to change back to gcc-5 back to compile caffe!

$sudo apt-get install gcc-4.9 g++-4.9

--> Set "priority=100" for gcc-4.9 and "priority=50" for gcc-5.
```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 100
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 50
```
--> Set "priority=100" for g++-4.9 and "priority=50" for g++-5.
```
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.9 100
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 50
```
--> Verify the priority settings using:
```
update-alternatives --query gcc
```
--> Start the matlab:
```
/media/elab/sdd/R2017a/bin/matlab
```
--> Go to the directory of matconvnet:
```
cd /media/elab/sdd/matlabInstall/matconvnet/
addpath matlab
```
--> Compile with gpu(without gpu always error when running!!!!!!)
```
vl_compilenn('enableGpu', true, 'enableCudnn', true)
```
