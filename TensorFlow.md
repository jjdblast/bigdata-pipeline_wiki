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

# MNIST
## Build TensorFlow Serving MNIST Example (24GB Minimum Docker Container)
* Errors during these build steps are likely due to not enough memory.  24GB+ is required.
```
cd $TENSORFLOW_SERVING_HOME

# Build the model, serving, and client components
bazel build //tensorflow_serving/example:mnist_export
bazel build //tensorflow_serving/example:mnist_inference_2
bazel build //tensorflow_serving/example:mnist_client
```

## Train and Deploy Example MNIST Model to TensorFlow Serving
```
cd $TENSORFLOW_SERVING_HOME/tensorflow
./configure   <-- Answer defaults (python location and gpu support)

# Train and Export Mnist Model to Path Monitored by TensorFlow Serving
# rm -rf $DATASETS_HOME/tensorflow/serving/mnist_model
# cd $TENSORFLOW_SERVING_HOME
$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_export --training_iteration=100 --export_version=1 $DATASETS_HOME/tensorflow/serving/mnist_model

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_export --training_iteration=1000 --export_version=2 $DATASETS_HOME/tensorflow/serving/mnist_model

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_export --training_iteration=10000 --export_version=3 $DATASETS_HOME/tensorflow/serving/mnist_model

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_export --training_iteration=100000 --export_version=4 $DATASETS_HOME/tensorflow/serving/mnist_model
```

## Start TensorFlow MNIST Serving Service (9090)
```
cd $TENSORFLOW_SERVING_HOME

# MNIST Inference with Dynamic Model Monitoring
nohup $TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_inference_2 --port=9090 $DATASETS_HOME/tensorflow/serving/mnist_model &
```

## Run MNIST Classifier Client (9090)
```
cd $TENSORFLOW_SERVING_HOME
$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_client --num_tests=1000 --server=localhost:9090 &
```

# Inception
## Build TensorFlow Serving Inception Example (24GB Minimum Docker Container)
* Errors during these build steps are likely due to not enough memory.  24GB+ is required.
```
cd $TENSORFLOW_SERVING_HOME

# Build inception model, inference serving, and client 
bazel build //tensorflow_serving/example:inception_export
bazel build //tensorflow_serving/example:inception_inference
bazel build //tensorflow_serving/example:inception_client
```

## Train and Deploy Example Inception Model to TensorFlow Serving
```
cd $TENSORFLOW_SERVING_HOME/tensorflow
./configure   <-- Answer defaults (python location and gpu support)

# Download checkpointed inception model to speed up training
wget http://download.tensorflow.org/models/image/imagenet/inception-v3-2016-03-01.tar.gz

# Train and Export Inception Model to Path Monitored by TensorFlow Serving
rm -rf $DATASETS_HOME/tensorflow/serving/inception_model


# Note:  the --checkpoint_dir must be local or you will get an error like the following:
#   "ValueError: Restore called with invalid save path model.ckpt-157585"
cd $TENSORFLOW_SERVING_HOME
$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/inception_export --checkpoint_dir=inception-v3 --export_dir=$DATASETS_HOME/tensorflow/serving/inception_model
```

## Start TensorFlow Inception Serving Service (9091)
```
cd $TENSORFLOW_SERVING_HOME

# Inception Inference
nohup $TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/inception_inference --port=9091 $DATASETS_HOME/tensorflow/serving/inception_model &
```

## Run Inception Classifier Client (9091)
```
cd $TENSORFLOW_SERVING_HOME
$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/inception_client --num_tests=1000 --server=localhost:9091 --image=$DATASETS_HOME/inception/cropped_panda.jpg
```

# NLP Sentence Prediction (PTB)
## Build Code
```
cd $TENSORFLOW_HOME
bazel build //tensorflow/models/rnn/ptb:ptb_word_lm
```

## Train Model
```
python ptb_word_lm.py --data_path=$DATASETS_HOME/ptb/simple-examples/data --model small
```

## Predict
```
TODO
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