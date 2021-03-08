# How-To-Install-TensorFlow-1.X-for-NVIDIA-RTX30-GPUs
How To Install TensorFlow 1.X for NVIDIA RTX30 GPUs (with CUDA11.1, cudnn8.0.4)

## My Environment
- Ubuntu 18.04.5
- CUDA 11.1
- cuDNN 8.0.4
- GTX 3090
- Python 3.6
- tensorflow-gpu 1.15.0</br>
## Step1: Download NVIDIA display driver, CUDA 11.1 and cuDNN 8.0.4
<a href="https://www.nvidia.com/Download/driverResults.aspx/163518/en-us" target="_blank">LINUX X64 (AMD64/EM64T) DISPLAY DRIVER</a><br/>
<a href="https://drive.google.com/drive/folders/1IzEd6XkBJIhVI7oMLLHDmaxvCzof0HVm?usp=sharing" target="_blank">nvidia-tensorflow dependency packages</a><br/>
<a href="https://developer.download.nvidia.com/compute/cuda/11.1.0/local_installers/cuda_11.1.0_455.23.05_linux.run" target="_blank">CUDA 11.1</a><br/>
<a href="https://developer.nvidia.com/rdp/cudnn-archive" target="_blank">cuDNN 8.0.4</a>
## Step2: Disable Nouveau nvidia driver on Ubuntu
 1) Open up terminal and enter the following commands:
 ```
 sudo gedit /etc/modprobe.d/blacklist.conf
 ```
 2) Insert follow lines to the blacklist.conf:
```
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```
 3) Save and exit
## Step3: Disable the Nouveau kernel driver
```
echo options nouveau modeset=0
sudo update-initramfs -u
```
Restart
```
sudo reboot
```
If executing the following command does not print anything, the disable of Nouveau is successful.
```
lsmod | grep nouveau
```
## Step4: Close the GUI and install zhcon(if Chinese garbled codes appears)
```
sudo systemctl set-default multi-user.target
sudo apt-get install zhcon
sudo adduser $(whoami) video
sudo reboot
```
## Step5: Install the graphics driver and CUDA 11.1(using runfile)
1) Uninstall the old drivers
```
sudo zhcon --utf8
sudo apt-get remove --purge nvidia*
sudo apt-get update
sudo apt-get install dkms build-essential linux-headers-generic
```
2) Install cuda_11.1.0_455.23.05_linux.run on Ubuntu
```
sudo chmod a+x cuda_11.1.0_455.23.05_linux.run
sudo ./cuda_11.1.0_455.23.05_linux.run
```
3) Type 'accept' on the protocol page
4) Go to the page showing options for installation(closing openGL)</br>
Options - Driver Options - Do not install any of the OpenGL - related driver files
![QQ截图20210308165332](https://user-images.githubusercontent.com/50128244/110297950-f3ad3d80-802e-11eb-9782-33f5051c8605.png)</br>
Done - Install
5) Open the GUI after installation.
```
sudo systemctl set-default graphical.target
sudo reboot
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;To check if the NVIDIA graphics driver was installed successfully.
```
nvidia-smi
```
## Step6: Configure paths
1) Open the .bashrc file 
```
sudo gedit ~/.bashrc
```
2) Add paths at the end</br>
The CUDA path is not added with a version number so that it does not need to be reconfigured when upgrading.
```
export CUDA_HOME=/usr/local/cuda
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${CUDA_HOME}/lib64
export PATH=${CUDA_HOME}/bin:${PATH}
export PATH="/home/<username>/anaconda3/install/bin:$PATH
```
3) Save and exit. Preserve.
```
source ~/. bashrc
```
4) Check for the version of CUDA installed
```
nvcc --version
cat /proc/driver/nvidia/version
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;OUTPUT: the version number
## Step7: Install cuDNN 8.0.4
1) Unzip the cuDNN Library for Linux(x86_64), and open the terminal in the path where the file is located.
```
sudo cp cuda/lib64/* /usr/local/cuda/lib64/
sudo cp cuda/include/* /usr/local/cuda/include/
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```
2) Check the cuDNN was configured successfully.
```
cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;OUTPUT: the version number
## Step8: Install Anaconda3, and create a virtual environment
```
sconda create -n name python=3.6.8
conda activate name
```
## Step9: Install tensorflow
1) Install nvidia-tensorflow dependency packages
```
sh requirements.sh
```
2) Verify that the library file exits</br>
![) X5WE%Q6F~FB7U2BJE{%$2](https://user-images.githubusercontent.com/50128244/110304126-2d357700-8036-11eb-8532-f9b40dca428d.png)
3) Check if tensorflow-gpu was installed successfully
```
python -c 'import tensorflow as tf; print(tf.__version__)'
```
