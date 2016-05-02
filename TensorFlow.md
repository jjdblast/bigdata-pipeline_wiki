## Configure TensorFlow (Defaults are Fine)
```
cd $TENSORFLOW_SERVING_HOME/tensorflow

./configure              
Please specify the location of python. [Default is /usr/bin/python]: 
Do you wish to build TensorFlow with GPU support? [y/N] 
No GPU support will be enabled for TensorFlow
Configuration finished
```

## Setup MNIST Classifier (Images of Numbers)
* Requires **24 GB Minimum Docker Container**
* Errors during these build steps are likely due to not enough memory.  
* 24 GB+ is required.  Please believe us!  :)
```
cd $TENSORFLOW_SERVING_HOME

### Build the model, serving, and client components
bazel build //tensorflow_serving/example:mnist_export
bazel build //tensorflow_serving/example:mnist_inference_2
bazel build //tensorflow_serving/example:mnist_client
```

### Start TensorFlow Serving with MNIST Model
* This will pick up the v1 MNIST Model that has already been training using 1000 training iterations  
```
cd $TENSORFLOW_SERVING_HOME

nohup $TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_inference_2 --port=9090 $DATASETS_HOME/tensorflow/serving/mnist_model &
```

### Perform MNIST Classification using v1 MNIST Model
* This will run 1000 tests against the v1 MNIST Model and output a metric that we're trying to minimize
* (This is kind of boring, but this will get more interesting down below... stick with us.)
```
cd $TENSORFLOW_SERVING_HOME
$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_client --num_tests=1000 --server=localhost:9090 &
```

### Train and Deploy v2 MNIST Model to TensorFlow Serving
* This will train and deploy the v2 model with 10,000 training iterations - an order of magnitude more than v1
* TensorFlow Serving will automatically pick up the new v2 MNIST Model
```
cd $TENSORFLOW_SERVING_HOME

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_export --training_iteration=10000 --export_version=2 $DATASETS_HOME/tensorflow/serving/mnist_model
```

### Perform MNIST Classification using v2 MNIST Model
* This will run the same number of tests (1000) as the prior run, but against the new v2 MNIST Model
* We should see the output metric decrease from the earlier run
```
cd $TENSORFLOW_SERVING_HOME
$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_client --num_tests=1000 --server=localhost:9090 &
```

## Inception (Images from ImageNet) Classifier
### Build TensorFlow Serving Inception Example (24GB Minimum Docker Container)
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