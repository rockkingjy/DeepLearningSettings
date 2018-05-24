* Install [Matlab](https://drive.google.com/open?id=1HGlRtgAd7DIbmuMlrmA9YFesnOAp49fE).
* Change the default shortcut: Home -> Preferences -> Keyboard/Shortcuts -> Active settings -> Windows Default Set.
* Change the color scheme: Home -> Preferences -> Colors -> Change the color.
* Install [Matconvenet](http://www.vlfeat.org/matconvnet/install/):

Install gcc4.9, to prevent "Missing symbole" problem:
```
sudo apt-get install gcc-4.9 g++-4.9
```
Set "priority=100" for gcc-4.9 and "priority=50" for gcc-5 and g++-5.
```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 100
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 50
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.9 100
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 50
```
Verify the priority settings using:
```
update-alternatives --query gcc
```
### Careful! Need to change back to set high priority to gcc-5 to compile caffe!
```
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 50
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 100
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.9 50
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 100
```
Start the matlab:
```
LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libstdc++.so.6 \
LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu \
/media/elab/sdd/R2017a/bin/matlab
```
Go to the directory of matconvnet, add the path to matlab:
```
cd /media/elab/sdd/matlabInstall/matconvnet/
addpath matlab
```
If just cpu:
```
vl_compilenn;
```
Run quick start examples:http://www.vlfeat.org/matconvnet/quick/

**Attention:** Before use the functions, should run:
```
vl_setupnn;
```
if not, will cause 'SCRIPT' error!!!
--------------------------
If using gpu:
```
vl_compilenn('enableGpu', true, 'cudaRoot', '/usr/local/cuda-8.0', 'enableCudnn', true)

```


