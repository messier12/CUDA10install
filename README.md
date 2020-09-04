# CUDA 10.0 installation on Ubuntu 18.04
I’ll start my story with saying that I had issues like three years ago with CUDA 8.0 and Ubuntu 16.04. This time I’ll be smarter and document the process for my future self.

1. Start terminal and remove any NVIDIA traces you may have on your machine.
```bash
$ sudo rm /etc/apt/sources.list.d/cuda*
$ sudo apt remove --autoremove nvidia-cuda-toolkit
$ sudo apt remove --autoremove nvidia-*
```
2. Setup the correct CUDA PPA
```bash
$ sudo apt update
$ sudo add-apt-repository ppa:graphics-drivers
$ sudo apt-key adv --fetch-keys  http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
$ sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda.list'
$ sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda_learn.list'
```

3. Install CUDA 10.0 packages

```bash
$ sudo apt update
$ sudo apt install cuda-10-0
$ sudo apt install libcudnn7
```
4. As the last step one need to specify PATH to CUDA in ‘.profile’ file. Open the file by running:
```bash
$ sudo vim ~/.profile
```
add the following lines at the end of the file:
```bash
# set PATH for cuda 10.1 installation
if [ -d "/usr/local/cuda-10.0/bin/" ]; then
   export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
   export LD_LIBRARY_PATH=/usr/local/cuda-10.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
fi
```
5. Restart and check the versions for the installation.

check CUDA version
```bash
$ nvcc --version
```
the output should look like this
```
nvcc  – versionnvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Wed_Apr_24_19:10:27_PDT_2019
Cuda compilation tools, release 10.1, V10.1.168
```
check NVIDIA driver
```bash
$ nvidia-smi
```
here is it's example output
```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 440.100      Driver Version: 440.100      CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1050    Off  | 00000000:01:00.0 Off |                  N/A |
| N/A   47C    P8    N/A /  N/A |    556MiB /  4042MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|    0      1773      G   /usr/lib/xorg/Xorg                           278MiB |
|    0      1953      G   /usr/bin/gnome-shell                         123MiB |
|    0      2349      G   ...uest-channel-token=14680595174548163996    56MiB |
|    0     16347      G   ...AAAAAAAAAAAACAAAAAAAAAA= --shared-files    93MiB |
+-----------------------------------------------------------------------------+
```
check libcudnn
```
$ /sbin/ldconfig -N -v $(sed ‘s/:/ /’ <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn
```
