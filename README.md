# setting-up-gpu-vm

These instructions are for installing CUDA through the repository instead of the .deb installation.

The following lines you can copy and paste to a terminal window. Press `Ctrl+Alt+T` to open a terminal window.

Remove any CUDA PPAs that may be setup and also remove the nvidia-cuda-toolkit if installed:

`sudo rm /etc/apt/sources.list.d/cuda*`<br />
`sudo apt remove --autoremove nvidia-cuda-toolkit`<br />
Recommended to also remove all NVIDIA drivers before installing new drivers:

`sudo apt remove --autoremove nvidia-*`
Then update the system:

`sudo apt update`
Recently, I just found out that the CUDA installation works with the graphics-drivers ppa so if you don't have it added, add it now:

`sudo add-apt-repository ppa:graphics-drivers/ppa`
Install the key:

`sudo apt-key adv --fetch-keys  http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub`
Add the repos:

`sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda.list'`

`sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda_learn.list'`
Update the system again:

`sudo apt update`
Install CUDA 10.1:

sudo apt install cuda-10-1
It should be installing the NVIDIA 418.40 drivers with it as those are what are listed in the repo. See: http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/

Install libcudnn7 7.5.1:

`sudo apt install libcudnn7`
Add the following lines to your ~/.profile file for CUDA 10.1

`# set PATH for cuda 10.1 installation
if [ -d "/usr/local/cuda-10.1/bin/" ]; then
    export PATH=/usr/local/cuda-10.1/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
fi`
Reboot the computer and check your settings when reboot is complete:

Check NVIDIA Cuda Compiler with `nvcc --version`:

nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Wed_Apr_24_19:10:27_PDT_2019
Cuda compilation tools, release 10.1, V10.1.168
Check libcudnn version /sbin/ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn:

terrance@terrance-ubuntu:~$ /sbin/ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn
    libcudnn.so.7 -> libcudnn.so.7.5.1
Check NVIDIA driver with nvidia-smi:

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
.run file install
By using the `sudo add-apt-repository ppa:graphics-drivers/ppa` you can install the 430.26 newest driver or any that suit your fancy.

Next, install the libcudnn7 by following:

Add the Repo:

`sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda_learn.list'`
Install the key:

sudo apt-key adv --fetch-keys  http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
Update the system:

sudo apt update
Install libcudnn7.5.1:

sudo apt install libcudnn7
Now download the cuda_10.1.105_418.39_linux.run from https://developer.nvidia.com/cuda-10.1-download-archive-base?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=runfilelocal

Then run the installer:

`sudo sh cuda_10.1.105_418.39_linux.run`
Type in accept and press enter on this screen:

┌──────────────────────────────────────────────────────────────────────────────┐
│  End User License Agreement                                                  │
│  --------------------------                                                  │
│                                                                              │
│                                                                              │
│  Preface                                                                     │
│  -------                                                                     │
│                                                                              │
│  The Software License Agreement in Chapter 1 and the Supplement              │
│  in Chapter 2 contain license terms and conditions that govern               │
│  the use of NVIDIA software. By accepting this agreement, you                │
│  agree to comply with all the terms and conditions applicable                │
│  to the product(s) included herein.                                          │
│                                                                              │
│                                                                              │
│  NVIDIA Driver                                                               │
│                                                                              │
│                                                                              │
│  Description                                                                 │
│                                                                              │
│  This package contains the operating system driver and                       │
│──────────────────────────────────────────────────────────────────────────────│
│ Do you accept the above EULA? (accept/decline/quit):                         │
│ accept                                                                       
Unselect the driver and then choose Install by using the arrow keys and space bar to move and select or unselect:

┌──────────────────────────────────────────────────────────────────────────────┐
│ CUDA Installer                                                               │
│ - [ ] Driver                                                                 │
│      [ ] 418.39                                                              │
│ + [X] CUDA Toolkit 10.1                                                      │
│   [X] CUDA Samples 10.1                                                      │
│   [X] CUDA Demo Suite 10.1                                                   │
│   [X] CUDA Documentation 10.1                                                │
│   Install                                                                    │
│   Options                                                                    │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│                                                                              │
│ Up/Down: Move | Left/Right: Expand | 'Enter': Select | 'A': Advanced options │
Wait for the install to finish, it might say errors during, but not to worry.

Add the following lines to your ~/.profile file for CUDA 10.1
`# set PATH for cuda 10.1 installation
if [ -d "/usr/local/cuda-10.1/bin/" ]; then
    export PATH=/usr/local/cuda-10.1/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
fi`
Reboot the system for the changes to take effect.
