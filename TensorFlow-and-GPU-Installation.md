# Docker Image
## Environment Variables
```
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
export BAZEL_VERSION=0.2.1
export TENSORFLOW_SERVING_VERSION=0.4.1
export TENSORFLOW_VERSION=0.7.1
```

## Install Bazel
```
export BAZEL_HOME=/root/bazel-$BAZEL_VERSION

cd ~
wget https://github.com/bazelbuild/bazel/releases/download/$BAZEL_VERSION/bazel-$BAZEL_VERSION-installer-linux-x86_64.sh
chmod +x bazel-$BAZEL_VERSION-installer-linux-x86_64.sh

./bazel-$BAZEL_VERSION-installer-linux-x86_64.sh --bin=$BAZEL_HOME/bin
rm bazel-$BAZEL_VERSION-installer-linux-x86_64.sh

...

export PATH=$PATH:$BAZEL_HOME/bin
```

## Install TensorFlow Serving
```
pip install --upgrade grpcio

apt-get update && apt-get install -y \
        build-essential \
        curl \
        git \
        libfreetype6-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        python-dev \
        python-numpy \
        python-pip \
        software-properties-common \
        swig \
        zip \
        zlib1g-dev

cd ~
git clone -b $TENSORFLOW_SERVING_VERSION --recurse-submodules https://github.com/tensorflow/serving tensorflow-serving-$TENSORFLOW_SERVING_VERSION

cd ~
git clone -b v$TENSORFLOW_VERSION --recurse-submodules https://github.com/tensorflow/tensorflow tensorflow-$TENSORFLOW_VERSION
```

## Build Components of TensorFlow Serving (24GB Minimum Docker Container)
* Errors during these build steps are likely due to not enough memory.  24GB+ is required.
```
cd $TENSORFLOW_SERVING_HOME

bazel build //tensorflow_serving/example:mnist_export
bazel build //tensorflow_serving/example:mnist_inference
bazel build //tensorflow_serving/example:mnist_client
```

## Train and Deploy Example Model to TensorFlow Serving
```
cd $TENSORFLOW_SERVING_HOME/tensorflow
./configure   <-- Answer defaults (python location and gpu support)

# [TODO:  Only if deploying new model] Remove any existing model
# rm -rf $WORK_HOME/tensorflow/serving/mnist_model/

# Train the model
# cd $TENSORFLOW_SERVING_HOME
# bazel-bin/tensorflow_serving/example/mnist_export $WORK_HOME/tensorflow/serving/mnist_model

# Export the model to a location monitored by TensorFlow Serving
# $TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_export $WORK_HOME/tensorflow/serving/mnist_model
```

## Start TensorFlow Serving Service
```
cd $TENSORFLOW_SERVING_HOME
nohup $TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_inference --port=9090 $DATASETS_HOME/tensorflow/serving/mnist_model/00000001 &
```

## Test TensorFlow Serving Service
```
cd $TENSORFLOW_SERVING_HOME
./bazel-bin/tensorflow_serving/example/mnist_client --num_tests=1000 --server=localhost:9090 &
```

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