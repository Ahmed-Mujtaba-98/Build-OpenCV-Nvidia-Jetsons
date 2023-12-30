# Configuration Guideline for Nvidia Jetson Xavier/Nano

This guide not only builds OpenCV with CUDA support, but also provides installation steps for other utilities, like ,
- "jtop" to monitor the resource usages (CPU, GPU, and memory)
- Deep learning libraries like PyTorch and Tensorflow.
- Mediapipe
- VSCode
- PyInstaller

> If you want to just build OpenCV with CUDA support, you can follow this [repository](https://github.com/JetsonHacksNano/buildOpenCV). 

## Prerequisites

This guide assumes that you have already built an image on Nvidia Jetson. 

## Step 01: Install pip
Install pip by using:
```bash
sudo apt install python3-pip
```
## Step 02: Install jtop
Install jtop to monitor the resource usages in Jetson by using this command:

```bash
sudo -H pip install --no-cache-dir -U jetson-stats
```
Run "jtop" from terminal and head to the info tab. There you can find all the information regarding the jetson. 

## Step 03: Build OpenCV with CUDA
Clone this repository by using:

```bash
git clone https://github.com/JetsonHacksNano/buildOpenCV
cd buildOpenCV
```

Change the following in the buildOpenCV.sh file.

 - ARCH_BIN
 - OPENCV_VERSION

You can find this information by using the "jtop" command in "info" tab. After updating the above-mentioned parameters, you can run the following command:
```bash
./buildOpenCV.sh |& tee openCV_build.log
```
After running the shell script, you may need to reboot the jetson. Head to the "info" tab by using the "jtop" command to validate OpenCV is successfully built with CUDA support.

**Note**
On Jetson nano, you may run into some memory issues, therefore, you can create a swap file to overcome such problems. To create a swap file, run the following commands:

```bash
git clone https://github.com/JetsonHacksNano/installSwapfile.git
cd installSwapfile
./installSwapFile.sh
```
Reboot jetson!


## Step 04: Install PyTorch and Torchvision [Optional]

To install torch and torchvision on jetson, you can follow the guidelines provided in this [link](https://qengineering.eu/install-pytorch-on-jetson-nano.html). Or, you can follow these commands to install torch==1.10.0:

```bash
# install the dependencies (if not already onboard)  
$ sudo apt-get install python3-pip libjpeg-dev libopenblas-dev libopenmpi-dev libomp-dev  
$ sudo -H pip3 install future  
$ sudo pip3 install -U --user wheel mock pillow  
$ sudo -H pip3 install testresources  
# above 58.3.0 you get version issues  
$ sudo -H pip3 install setuptools==58.3.0  
$ sudo -H pip3 install Cython  
# install gdown to download from Google drive  
$ sudo -H pip3 install gdown  
# download the wheel  
$ gdown https://drive.google.com/uc?id=1TqC6_2cwqiYacjoLhLgrZoap6-sVL2sd  
# install PyTorch 1.10.0  
$ sudo -H pip3 install torch-1.10.0a0+git36449ea-cp36-cp36m-linux_aarch64.whl  
# clean up  
$ rm torch-1.10.0a0+git36449ea-cp36-cp36m-linux_aarch64.whl
```

For torchvision;

```bash
# the dependencies  
$ sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev  
$ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev  
$ sudo pip3 install -U pillow  
# install gdown to download from Google drive, if not done yet  
$ sudo -H pip3 install gdown  
# download TorchVision 0.10.0  
$ gdown https://drive.google.com/uc?id=1Q2NKBs2mqkk5puFmOX_pF40yp7t-eZ32  
# install TorchVision 0.10.0  
$ sudo -H pip3 install torchvision-0.10.0a0+300a8a4-cp36-cp36m-linux_aarch64.whl  
# clean up  
$ rm torchvision-0.10.0a0+300a8a4-cp36-cp36m-linux_aarch64.whl
```

## Step 05: Install TensorFlow [Optional]

To install tensorflow==2.4.1 on jetson, you can follow the guidelines provided in this [link](https://qengineering.eu/install-tensorflow-2.4.0-on-jetson-nano.html). Or you can follow these commands:

```bash
# get a fresh start  
$ sudo apt-get update  
$ sudo apt-get upgrade  
# install pip and pip3  
$ sudo apt-get install python-pip python3-pip  
# remove old versions, if not placed in a virtual environment (let pip search for them)  
$ sudo pip uninstall tensorflow  
$ sudo pip3 uninstall tensorflow  
# install the dependencies (if not already onboard)  
$ sudo apt-get install gfortran  
$ sudo apt-get install libhdf5-dev libc-ares-dev libeigen3-dev  
$ sudo apt-get install libatlas-base-dev libopenblas-dev libblas-dev  
$ sudo apt-get install liblapack-dev  
$ sudo -H pip3 install Cython==0.29.21  
# install h5py with Cython version 0.29.21 (± 6 min @1950 MHz)  
$ sudo -H pip3 install h5py==2.10.0  
$ sudo -H pip3 install -U testresources numpy  
# upgrade setuptools 39.0.1 -> 53.0.0  
$ sudo -H pip3 install --upgrade setuptools  
$ sudo -H pip3 install pybind11 protobuf google-pasta  
$ sudo -H pip3 install -U six mock wheel requests gast  
$ sudo -H pip3 install keras_applications --no-deps  
$ sudo -H pip3 install keras_preprocessing --no-deps  
# install gdown to download from Google drive  
$ sudo -H pip3 install gdown  
# download the wheel  
$ gdown https://drive.google.com/uc?id=1DLk4Tjs8Mjg919NkDnYg02zEnbbCAzOz  
# install TensorFlow (± 12 min @1500 MHz)  
$ sudo -H pip3 install tensorflow-2.4.1-cp36-cp36m-linux_aarch64.whl
```

## Step 06: Install MediaPipe [Optional]

To install mediapipe on jetson, you can follow the guidelines provided in this [link](https://github.com/JetsonHacksNano/buildOpenCV/tree/master). Or you can follow these commands:

```bash
sudo apt update

# install system packages required by TensorFlow:
sudo apt-get update
sudo apt-get install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran

# install and upgrade pip3
sudo apt-get install python3-pip
sudo pip3 install -U pip testresources setuptools==49.6.0

# install the Python package dependencies
sudo pip3 install -U --no-deps numpy==1.19.4 future==0.18.2 mock==3.0.5 keras_preprocessing==1.1.2 keras_applications==1.0.8 gast==0.4.0 protobuf pybind11 cython pkgconfig
sudo env H5PY_SETUP_REQUIRES=0 pip3 install -U h5py==3.1.0

# Now we need to install opencv-python:
sudo apt-get install python3-opencv 
sudo apt-get remove python3-opencv

# Now all the Prerequisites and Dependencies are installed. 
# Now let’s get the mediapipe source code:
cd
git clone https://github.com/google/mediapipe.git
cd mediapipe

sudo apt-get install -y libopencv-core-dev  libopencv-highgui-dev libopencv-calib3d-dev libopencv-features2d-dev libopencv-imgproc-dev libopencv-video-dev
sudo chmod 744 setup_opencv.sh
./setup_opencv.sh

sudo pip3 install opencv_contrib_python
sudo apt install curl

# so guys the medipipe-bin has changed so the installation is little  different from the video only this step has some changes.
# Do as I say in this github. Now first you need to download this folder of medipipe-bin from this link.
https://drive.google.com/file/d/1lHr9Krznst1ugLF_ElWGCNi_Y4AmEexx/view?usp=sharing

# After downloading the zip file unzip it by going to where it is been download in your jetson , mostly in the Downloads folder     
cd Downloads
sudo apt install unzip
unzip mediapipe-bin.zip
cd mediapipe-bin
   
sudo pip3 install numpy-1.19.4-cp36-none-manylinux2014_aarch64.whl mediapipe-0.8.5_cuda102-cp36-none-linux_aarch64.whl
pip3 install dataclasses
```

**Attention:**

Mediapipe installation may disable OpenCV with CUDA support, therefore, you may want to rebuilt OpenCV with CUDA support by following **Step 03**.

## Step 07: Install Visual Studio Code [Optional]
Download the stable version (stable.deb) from this [link](https://drive.worksmobile.com/#/group/QDEwMDEwMDAwMDM5MjU0OTh8MzQ3MjUxMDIwMDczNTQ5NTE3NnxEfDM0NzI0NzA0NTM5Mjg2NzQwNTY) or use this command:

```bash
wget https://update.code.visualstudio.com/1.50.0/linux-deb-arm64/stable -O stable.deb
```

Use this command for vscode installation on jetson:
```bash
sudo dpkg -i stable.deb
```

## Step 08: Install PyInstaller for Packaging [Optional]
Normal installation of PyInstaller via pip sometimes doesn't work with opencv-python library on jetson, therefore, the solution is to downgrade "pyinstaller" and "pyinstaller-hooks-contrib" versions

You can use the following versions:

```bash
pip3 install pyinstaller==4.2 
pip3 install pyinstaller-hooks-contrib==2021.2 
pyinstaller --onefile --paths="/usr/lib/python3.6/dist-packages/cv2/python-3.6" target.py
```

Best of Luck :)
