# Setup GPU Host Machine

http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#ubuntu-installation

## Remove Existing Nouveau Video Driver
* Create a file at /etc/modprobe.d/blacklist-nouveau.conf with the following contents:
```
blacklist nouveau
options nouveau modeset=0
```

* Regenerate the kernel initramfs:
```
sudo update-initramfs -u
```

## Installing CUDA Toolkit
```
wget http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda_7.5.18_linux.run
```

## Perform the following steps to install CUDA and verify the installation.

(http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#runfile)

* Run the installer silently to install with the default selections (implies acceptance of
the EULA):
```
local-gpu$ sh cuda_7.5.18_linux.run --silent
```

## Set LD_LIBRARY_PATH variables:
```
local-gpu$ export PATH=/usr/local/cuda-7.5/bin:$PATH
local-gpu$ export LD_LIBRARY_PATH=/usr/local/cuda-7.5/lib64:$LD_LIBRARY_PATH
```

7. Install a writable copy of the samples then build and run the deviceQuery sample
```
local-gpu$ cuda-install-samples-7.5.sh ~
local-gpu$ cd ~/NVIDIA_CUDA-Samples_7.5/1_Utilities/deviceQuery
local-gpu$ make
local-gpu$ ./deviceQuery
```

## Installing cuDNN

(Download from nvidia directly - requires registration and approval) area 

* Something like the following (not exactly... nvidia changes this sometimes)
```
tar xvzf cudnn-7.5-linux-x64-v2.tgz
sudo cp cudnn-7.5-linux-x64-v2/cudnn.h /usr/local/cuda/include
sudo cp cudnn-7.5-linux-x64-v2/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

## Start GPU-enabled Docker
```
local-gpu$ <pipeline-install-dir>/bin/docker/docker-start-container-gpu.sh
```
* Uninstall Default CPU TensorFlow and Install GPU TensorFlow

https://www.tensorflow.org/versions/r0.7/get_started/os_setup.html#pip-installation

* TODO:  `pip uninstall` the CPU version of TensorFlow
```
pip uninstall ...
``` 
* TODO:  `pip install` the GPU version of TensorFlow
```
pip install ...
```