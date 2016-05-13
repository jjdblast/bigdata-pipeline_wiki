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
* **24 GB+ is required.**  Please believe us!  :)

### Build the Inference and Client Components
```
cd $TENSORFLOW_SERVING_HOME

bazel build //tensorflow_serving/example:mnist_inference_2

bazel build //tensorflow_serving/example:mnist_client
```

### Build the Export Component
* This is just building the Export Component - not actually building and exporting the model
* This command takes a while (10-15 mins), so please be patient.
```
cd $TENSORFLOW_SERVING_HOME

bazel build //tensorflow_serving/example:mnist_export
```

### Train and Deploy v1 MNIST Model to TensorFlow Serving
* This will train and deploy the v1 model with 1,000 training iterations
* TensorFlow Serving will automatically pick up the v1 MNIST Model
* This step will take a while (3-5 mins), so please be patient
```
cd $TENSORFLOW_SERVING_HOME

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_export --training_iteration=1000 --export_version=1 $DATASETS_HOME/tensorflow/serving/mnist_model
```

### Start TensorFlow Serving with MNIST Model (Port 9090)
* This will pick up the v1 MNIST Model that has been training using 1,000 training iterations
```
cd $TENSORFLOW_SERVING_HOME

nohup $TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_inference_2 --port=9090 $DATASETS_HOME/tensorflow/serving/mnist_model > nohup-mnist.out &
```
* Verify that TensorFlow Serving has found the v1 MNIST Model `mnist_model/00000001`
```
tail -f nohup-mnist.out 
tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:85] Found a servable {name: default version: 1} at path /root/pipeline/datasets/tensorflow/serving/mnist_model/00000001
tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:149] Aspiring 1 versions for servable default
```
* `ctrl-c` to exit the `tail` log

### Perform MNIST Classification using v1 MNIST Model (Port 9090)
* This will run 1000 tests against the v1 MNIST Model and output an `Inference error rate` metric that we'll try to minimize in the next step
* (This is kind of boring, but this will get more interesting down below... stick with us.)
```
cd $TENSORFLOW_SERVING_HOME

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_client --num_tests=1000 --server=localhost:9090
...
Inference error rate: 13.1%
```

### Train and Deploy v2 MNIST Model to TensorFlow Serving
* This will train and deploy the v2 model with 10,000 training iterations - an order of magnitude more than v1
* TensorFlow Serving will automatically pick up the new v2 MNIST Model
* This step will take a while (3-5 mins), so please be patient
```
cd $TENSORFLOW_SERVING_HOME

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_export --training_iteration=10000 --export_version=2 $DATASETS_HOME/tensorflow/serving/mnist_model
```

* Verify that TensorFlow Serving has found the new v2 MNIST Model `mnist_model/00000002`
```
tail -f nohup-mnist.out 
tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:149] Aspiring 1 versions for servable default
tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:45] Polling the file system to look for a servable path under: /root/pipeline/datasets/tensorflow/serving/mnist_model
tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:85] Found a servable {name: default version: 2} at path /root/pipeline/datasets/tensorflow/serving/mnist_model/00000002
```
* `ctrl-c` to exit the `tail` log

### Perform MNIST Classification using v2 MNIST Model (Port 9090)
* This will run the same number of tests (1000) as the prior run, but against the new v2 MNIST Model
* `Inference error rate` should have **decreased** from the earlier run
```
cd $TENSORFLOW_SERVING_HOME

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/mnist_client --num_tests=1000 --server=localhost:9090
...
Inference error rate: 9.5%
```

## Setup Inception Model Classifier

### Build TensorFlow Serving Inception Example
* Requires **24 GB Minimum Docker Container**
* Errors during these build steps are likely due to not enough memory.  
* **24 GB+ is required.**  Please believe us!  :)

### Build the Inference and Client Components
```
cd $TENSORFLOW_SERVING_HOME

# Build Inference and Client Components
bazel build //tensorflow_serving/example:inception_inference

bazel build //tensorflow_serving/example:inception_client
```

### Build the Export Component
* This is just building the Export Component - not actually building and exporting the model
```
cd $TENSORFLOW_SERVING_HOME

bazel build //tensorflow_serving/example:inception_export
```

## Train and Deploy the Inception Model to TensorFlow Serving
* Because of the size of the Inception Model, we need to download a checkpoint version to start with
* The `--checkpoint_dir` must be local to where we build the model or else we see an error like the following:
```
"ValueError: Restore called with invalid save path model.ckpt-157585"
```
* Follow these instructions and you'll be fine
```
cd $TENSORFLOW_SERVING_HOME

# Download the Inception Model Checkpoint to Train On
wget http://download.tensorflow.org/models/image/imagenet/inception-v3-2016-03-01.tar.gz

tar -xvzf inception-v3-2016-03-01.tar.gz

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/inception_export --checkpoint_dir=inception-v3 --export_dir=$DATASETS_HOME/tensorflow/serving/inception_model
```

### Start TensorFlow Serving with Inception Model (Port 9091)
```
$MYAPPS_HOME/tensorflow/start-tensorflow-inception-serving-service.sh
```

* Verify that TensorFlow Serving found v00157585 Inception Model `inception_model/00157585`
```
tail -f $LOGS_HOME/tensorflow/inception/nohup-inception.out
tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:149] Aspiring 1 versions for servable default
tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:45] Polling the file system to look for a servable path under: /root/pipeline/datasets/tensorflow/serving/inception_model
tensorflow_serving/sources/storage_path/file_system_storage_path_source.cc:85] Found a servable {name: default version: 157585} at path /root/pipeline/datasets/tensorflow/serving/inception_model/00157585
```

### Perform Image Classification using Inception Model (Port 9091)
```
cd $TENSORFLOW_SERVING_HOME

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/inception_client --server=localhost:9091 --image=$DATASETS_HOME/inception/cropped_panda.jpg
```

### Classify Your Own Image using Inception Model (Port 9091)
```
wget <my-remote-image-url>

cd $TENSORFLOW_SERVING_HOME

$TENSORFLOW_SERVING_HOME/bazel-bin/tensorflow_serving/example/inception_client --server=localhost:9091 --image=<my-local-image-filename>
```

## TODO:  Quantize to 8-bit for Faster Inference (v0.9+)
* http://www.kdnuggets.com/2016/05/how-quantize-neural-networks-tensorflow.html

* This will produce a new model that runs the same operations as the original, but with eight bit calculations internally, and all weights quantized as well. If you look at the file size, you’ll see it’s about a quarter of the original (23MB versus 91MB). You can still run this model using exactly the same inputs and outputs though, and you should get equivalent results. Here’s an example:
```
curl http://download.tensorflow.org/models/image/imagenet/inception-2015-12-05.tgz -o /tmp/inceptionv3.tgz
tar xzf /tmp/inceptionv3.tgz -C /tmp/
bazel build tensorflow/contrib/quantization/tools:quantize_graph
bazel-bin/tensorflow/contrib/quantization/tools/quantize_graph \
--input=/tmp/classify_image_graph_def.pb \
--output_node_names="softmax" --output=/tmp/quantized_graph.pb \
--mode=eightbit
```

* This runs the newly-quantized graph and outputs a very similar answer to the original
```
bazel build tensorflow/examples/label_image:label_image
bazel-bin/tensorflow/examples/label_image/label_image \
--input_graph=/tmp/quantized_graph.pb \
--input_width=299 \
--input_height=299 \
--mean_value=128 \
--std_value=128 \
--input_layer_name="Mul:0" \
--output_layer_name="softmax:0"
```


## TODO:  NLP Sentence Prediction with PTB
### Build Code
```
cd $TENSORFLOW_HOME
bazel build //tensorflow/models/rnn/ptb:ptb_word_lm
```

### Train Model
```
python ptb_word_lm.py --data_path=$DATASETS_HOME/ptb/simple-examples/data --model small
```

### Predict Sentence
```
TODO
```