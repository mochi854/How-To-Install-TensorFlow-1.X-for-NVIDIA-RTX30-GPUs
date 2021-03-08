# How-To-Install-TensorFlow-1.X-for-NVIDIA-RTX30-GPUs
How To Install TensorFlow 1.X for NVIDIA RTX30 GPUs (with CUDA11.1,cudnn8.0.4)

## My Environment
- Ubuntu 18.04.5
- CUDA 11.1
- cuDNN 8.0.4
- GTX 3090
- Python 3.6
- tensorflow-gpu 1.15.0

## Step1: Download CUDA 11.1 and cuDNN 8.0.4
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
4) 
