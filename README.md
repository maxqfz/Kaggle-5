# Kaggle-5

### Полезные ссылки
* Автор kaggle: https://github.com/pfillard/tpx-kaggle-dsb2017
* Как установить Cuda и CudNN: https://medium.com/@ikekramer/installing-cuda-8-0-and-cudnn-5-1-on-ubuntu-16-04-6b9f284f6e77
* Датасеты https://www.kaggle.com/c/data-science-bowl-2017/data

### Необходимые файлы:

#### Cuda 8
* https://developer.nvidia.com/cuda-80-ga2-download-archive
* Сама CUDA - https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-deb
* Патч к ней - https://developer.nvidia.com/compute/cuda/8.0/Prod2/patches/2/cuda-repo-ubuntu1604-8-0-local-cublas-performance-update_8.0.61-1_amd64-deb

#### CudNN 5.1
* Здесь https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v5.1/prod_20161129/8.0/cudnn-8.0-linux-x64-v5.1-tgz
* Либо здесь https://github.com/RashmiTiwari132/CUDNN/raw/master/cudnn-8.0-linux-x64-v5.1.tgz

#### Bazel 0.4.2
* Здесь https://github.com/bazelbuild/bazel/releases/download/0.4.2/bazel-0.4.2-installer-linux-x86_64.sh

#### Патченый Tensorflow
* https://github.com/pfillard/tensorflow/tree/r1.0_relu1

### Подготовка окружения:

#### Cuda Toolkit
```
sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-cublas-performance-update_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
```

#### CudNN
```
tar xvzf cudnn-8.0-linux-x64-v5.1.tgz
sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include/
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

#### NVIDIA CUDA Profile Tools Interface
```
sudo apt-get install libcupti-dev
```

#### Установка переменных сред
```
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64"
export CUDA_HOME=/usr/local/cuda
```

#### Python
```
sudo apt-get install python3 python3-numpy python3-dev python3-pip python3-wheel python3-mock
```

#### Пакеты Python
```
sudo pip3 install numpy
sudo pip3 install scipy
sudo pip3 install xgboost
sudo pip3 install scikit-image
sudo pip3 install SimpleITK
sudo pip3 install h5py
sudo pip3 install argparse
```

#### Bazel
```
chmod +x bazel-0.4.2-installer-linux-x86_64.sh
sudo ./bazel-0.4.2-installer-linux-x86_64.sh
```

### Сборка и установка Tensorflow
```
./configure
```
На первый вопрос указываем путь к третьему питону, далее просто Enter до вопроса про поддержку CUDA. На него отвечаем Y и нажимаем Enter до конца.

```
bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
sudo pip install /tmp/tensorflow_pkg/tensorflow-1.0.0rc0*
```

### Запуск кеггла
...