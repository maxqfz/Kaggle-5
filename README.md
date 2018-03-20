# Kaggle-5

### Полезные ссылки
* Автор kaggle: https://github.com/pfillard/tpx-kaggle-dsb2017
* Как установить Cuda и CudNN: https://medium.com/@ikekramer/installing-cuda-8-0-and-cudnn-5-1-on-ubuntu-16-04-6b9f284f6e77
* Датасеты https://www.kaggle.com/c/data-science-bowl-2017/data

### Необходимые файлы:

#### Cuda 8
*https://developer.nvidia.com/cuda-80-ga2-download-archive
*Сама CUDA - https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-deb
*Патч к ней - https://developer.nvidia.com/compute/cuda/8.0/Prod2/patches/2/cuda-repo-ubuntu1604-8-0-local-cublas-performance-update_8.0.61-1_amd64-deb

#### CudNN 5.1
*Здесь https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v5.1/prod_20161129/8.0/cudnn-8.0-linux-x64-v5.1-tgz
*Либо здесь https://github.com/RashmiTiwari132/CUDNN/raw/master/cudnn-8.0-linux-x64-v5.1.tgz

#### Tensorflow


### Подготовка окружения:

**Cuda Toolkit**
```
sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-cublas-performance-update_8.0.61-1_amd64-deb
sudo apt-get install cuda
```

**CudNN**
```
tar xvzf cudnn-8.0-linux-x64-v5.1-ga.tgz
sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include/
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

**Python**
```
sudo apt-get install python3 python3-numpy python3-dev python3-wheel python3-mock
```

**Python dependencies**
```
sudo pip3 install numpy
sudo pip3 install scipy
sudo pip3 install xgboost
sudo pip3 install scikit-image
sudo pip3 install SimpleITK
sudo pip3 install h5py
sudo pip3 install argparse
```

**Other dependencies**
```
sudo apt-get install libcupti-dev
```

### Установка Tensorflow













2. установка tensorflow r1.0_relu1 (здесь проблемы)
  - https://github.com/pfillard/tensorflow/tree/r1.0_relu1
  - https://www.tensorflow.org/install/install_sources
  - для того чтобы установить нужна програмка `bazel-0.4.5-installer-linux-x86_64.sh` качал через релиз на гите https://github.com/bazelbuild/bazel/releases/tag/0.4.5 (у меня получилось скачать только через свой пк, выложить на яндекс диск и скачать на сервер через `wget link -O bazel.zip` и потом `unzip bazel.zip`. Этот сайт поможет скачать на яндекс диск через ссылкy https://getfile.dokpub.com/yandex/)
  - кратко: качаем с гита, переходим на ветку, конфигурируем `./configure`, дальше надо скомпилировать `.whl` файл через `bazel build ...` и установить библиотеку через `pip3 `
  - при компиляции возникает ошибка: 
  ```
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
INFO: Found 1 target...
[285 / 2,349] Still waiting for 200 jobs to complete:
Server terminated abruptly (error code: 14, error message: '', log file: '/root/.cache/bazel/_bazel_root/4d4b5309430ecf8c638c52fba28ae6e2/server/jvm.out')
  ```