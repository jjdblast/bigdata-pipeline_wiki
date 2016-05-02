# Docker Image
## Environment Variables
```
export JAVA_HOME=/usr/lib/jvm/java-8-oracle
export BAZEL_VERSION=0.2.1
export TENSORFLOW_SERVING_VERSION=0.4.1
export TENSORFLOW_VERSION=0.8.0rc0
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
