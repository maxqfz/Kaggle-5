# Kaggle-5

### Полезные ссылки
* Автор kaggle: https://github.com/pfillard/tpx-kaggle-dsb2017
* Как установить Cuda и CudNN: https://medium.com/@ikekramer/installing-cuda-8-0-and-cudnn-5-1-on-ubuntu-16-04-6b9f284f6e77
* Датасеты https://www.kaggle.com/c/data-science-bowl-2017/data
* Собранное окружение для ДВК лежит у нас в гите в папке environment (архив .tar.gz, разбитый на 5 частей)

### Необходимые файлы:

#### Cuda 8
* https://developer.nvidia.com/cuda-80-ga2-download-archive
* Сама CUDA - https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-deb (для x64)
* Патч к ней - https://developer.nvidia.com/compute/cuda/8.0/Prod2/patches/2/cuda-repo-ubuntu1604-8-0-local-cublas-performance-update_8.0.61-1_amd64-deb (для x64)

#### CudNN 5.1
* Здесь https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v5.1/prod_20161129/8.0/cudnn-8.0-linux-x64-v5.1-tgz (для x64)
* Либо здесь https://github.com/RashmiTiwari132/CUDNN/raw/master/cudnn-8.0-linux-x64-v5.1.tgz (для x64)
* Либо здесь https://github.com/maxqfz/Kaggle-5/raw/master/packages/cudnn-8.0-linux-ppc64le-v5.1.tgz (для PowerPC)

#### Bazel 0.4.2
* Здесь https://github.com/bazelbuild/bazel/releases/download/0.4.2/bazel-0.4.2-installer-linux-x86_64.sh
* Исходник https://github.com/maxqfz/Kaggle-5/raw/master/packages/bazel-0.4.2-dist.zip

#### Патченый Tensorflow
* https://github.com/pfillard/tensorflow/tree/r1.0_relu1
* Собранный для x64, CC3.0 - https://github.com/maxqfz/Kaggle-5/raw/master/packages/tensorflow-1.0.0rc0-cp35-cp35m-linux_x86_64.whl
* Собранный для PowerPC, CC6.0 - https://github.com/maxqfz/Kaggle-5/raw/master/packages/tensorflow-1.0.0rc0-cp36-cp36m-linux_ppc64le.whl

### Подготовка окружения (на примере x64, аналогично только с другими пакетами для PowerPC):

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
*Перед запуском кеггла на ДВК установите переменное окружение командой `source ~/Kaggle5/2setenvvars.source`*
Запуск кеггла - `python3 -m ~/Kaggle5/tpx-kaggle-dsb2017/predict.py -i [CSV с сэмплами] -d [Папка для конв. серий]`:
* -i это CSV со столбцами id, cancer, где id - названия сконверченных в пару (.mhd, .raw) серий, cancer - любое число (0.5 обычно)
* -d путь до папки со сконверченными сериями
* Результат в ./predict_result/predictions.csv