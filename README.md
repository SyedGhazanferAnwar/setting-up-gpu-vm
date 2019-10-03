# setting-up-gpu-vm
## Ubuntu 18.04, CUDA 10.1, libcudnn 7.5.1 and NVIDIA 418.67 drivers:
These instructions are for installing CUDA through the repository instead of the .deb installation.The following lines you can copy and paste to a terminal window. Press `Ctrl+Alt+T` to open a terminal window.
Remove any CUDA PPAs that may be setup and also remove the nvidia-cuda-toolkit if installed:

`sudo rm /etc/apt/sources.list.d/cuda*`<br />
`sudo apt remove --autoremove nvidia-cuda-toolkit`<br />
Recommended to also remove all NVIDIA drivers before installing new drivers:<br />
`sudo apt remove --autoremove nvidia-*`<br />

Then update the system:<br />
`sudo apt update`

Recently, I just found out that the CUDA installation works with the graphics-drivers ppa so if you don't have it added, add it now:

`sudo add-apt-repository ppa:graphics-drivers/ppa`<br />
Install the key:

`sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub`
Add the repos:

`sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda.list'`

```
sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda_learn.list'
```
<br />
Update the system again:

`sudo apt update`<br />
Install CUDA 10.1:

`sudo apt install cuda-10-1`<br />
NOTE:If above command shows error then try  `sudo aptitude install cuda-10-1` <br />
It should be installing the NVIDIA 418.40 drivers with it as those are what are listed in the repo. See: http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/

Install libcudnn7 7.5.1:

`sudo apt install libcudnn7`<br />
Add the following lines to your ~/.profile file for CUDA 10.1 (`nano ~/.profile` and append the below code at last line)

```
# set PATH for cuda 10.1 installation
if [ -d "/usr/local/cuda-10.1/bin/" ]; then
    export PATH=/usr/local/cuda-10.1/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
fi
```
<br />
Reboot the computer and check your settings when reboot is complete:
```
sudo reboot
```
<br />

Check NVIDIA Cuda Compiler with `nvcc --version`:
```
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Wed_Apr_24_19:10:27_PDT_2019
Cuda compilation tools, release 10.1, V10.1.168
```
Check libcudnn version `/sbin/ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn:`

`terrance@terrance-ubuntu:~$ /sbin/ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn
    libcudnn.so.7 -> libcudnn.so.7.5.1`
Check NVIDIA driver with nvidia-smi:
```
terrance@terrance-ubuntu:~$ nvidia-smi 
Sat Jun  1 09:38:07 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.67       Driver Version: 418.67       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 750 Ti  On   | 00000000:02:00.0  On |                  N/A |
| 40%   38C    P0     2W /  38W |    116MiB /  2000MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      2216      G   /usr/lib/xorg/Xorg                           110MiB |
|    0      2542      G   compton                                        1MiB |
+-----------------------------------------------------------------------------+
```


https://askubuntu.com/questions/1077061/how-do-i-install-nvidia-and-cuda-drivers-into-ubuntu
