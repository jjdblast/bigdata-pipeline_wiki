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
pip install grpcio

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
git clone -b $TENSORFLOW_SERVING_VERSION --recurse-submodules https://github.com/tensorflow/serving
```

## Train and Deploy Example Model to TensorFlow Serving
```
cd $TENSORFLOW_SERVING_HOME/tensorflow
./configure   <-- Answer defaults (python location and gpu support)

cd $TENSORFLOW_SERVING_HOME
bazel build //tensorflow_serving/example:mnist_export

rm -rf $WORK_HOME/tensorflow/serving/mnist_model/
$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_export $WORK_HOME/tensorflow/serving/mnist_model
```

## Start TensorFlow Serving Service
```
nohup $TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_inference --port=9000 $WORK_HOME/tensorflow/serving/mnist_model/00000001 &
```

## Test TensorFlow Serving Service
```
$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_client --num_tests=1000 --server=localhost:9000 &
```

# Setup GPU Host Machine
## Installing CUDA Toolkit

```
wget http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda_7.5.18_linux.run
```

## Perform the following steps to install CUDA and verify the installation.

(http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#runfile)

* Run the installer silently to install with the default selections (implies acceptance of
the EULA):
```
sh cuda_7.5.18_linux.run --silent
```

## 4. Create an xorg.conf file to use the NVIDIA GPU for display:
##$ sudo nvidia-xconfig
## 5. Reboot the system to load the graphical interface.
## 6. Set up the development environment by modifying the PATH and

Set LD_LIBRARY_PATH variables:
$ export PATH=/usr/local/cuda-7.5/bin:$PATH
$ export LD_LIBRARY_PATH=/usr/local/cuda-7.5/lib64:$LD_LIBRARY_PATH

7. Install a writable copy of the samples then build and run the deviceQuery sample
$ cuda-install-samples-7.5.sh ~
$ cd ~/NVIDIA_CUDA-Samples_7.5/1_Utilities/deviceQuery
$ make
$ ./deviceQuery

## Installing cuDNN

(Download from nvidia directly - requires registration and approval) area 

tar xvzf cudnn-6.5-linux-x64-v2.tgz
sudo cp cudnn-6.5-linux-x64-v2/cudnn.h /usr/local/cuda/include
sudo cp cudnn-6.5-linux-x64-v2/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*

## Start GPU-enabled Docker
```
local-laptop$ <pipeline-install-dir>/bin/docker/docker-start-container-gpu.sh
```
