# Run TensorFlow python code on an Alma GPU

TensorFlow is a popular machine learning library that can be used to train and run deep learning models. TensorFlow can be run on a GPU to speed up training and inference. This guide will show you how to run TensorFlow code on an Alma GPU.

Getting the versions to match is tricky, this recipe works on 11th July 2024 when CUDA 11.1 on Alma at /opt/software/compilers/cuda/11.1 is the most recent CUDA version.  The versions of TensorFlow and CUDA are changing rapidly so you may need to adjust the versions to get them to work together.

## Log onto an interactive session on Alma

```shell
ssh username@alma.icr.ac.uk
srun --pty --mem=10GB -c 1 -t 30:00:00 -p interactive bash
```

## Create a mamba environment to work in for compatible versions:
We have cuda 11.1 installed so we need:  
[Versions conpatibility link](https://www.tensorflow.org/install/source#gpu)  
| Version | Python | version | Compiler | Build tools | cuDNN | CUDA |  
| ------- | ------ | ------- | -------- | ----------- | ----- | ---- |  
| tensorflow-2.4.0 | 3.6-3.8 | GCC | 7.3.1 | Bazel 3.1.0 | 8.0 | 11.0 |  

```shell
mamba create -n my-tensorflow -c conda-forge python=3.7 cudatoolkit=11.2.2 cudnn=8.1.0.77
mamba activate my-tensorflow
mamba install conda-forge::tensorflow
mamba install conda-forge::tensorflow-gpu
```

## Check you have the versions of python and TensorFlow you think you have
```shell
which python
python --version
python -m pip show tensorflow
python -c "import sys; print('\n'.join(sys.path))"
```

## Navigate somewhere sensible
```shell
mkdir /path/to/your/code
cd /path/to/your/code
```

## Create a python script to run TensorFlow code
Create a file `/path/to/your/code/python_script.py` with the following content:
```python
import sys
import tensorflow as tf

print("Starting Cuda-GPU-TensorFlow Test")
print('\n'.join(sys.path))
print(tf.__version__)

from tensorflow.python.client import device_lib
local_device_protos = device_lib.list_local_devices()
num_gpus = 0
for x in local_device_protos:    
    print(x)
    if 'GPU' in x.device_type:
        num_gpus += 1
print('Num GPUs Available: ', num_gpus)
```

## Create a batch script to run the python script
Create a file `/path/to/your/code/sbatch_script.sh` with the following content:
```shell
#!/bin/sh
#SBATCH -J "tf_test"
#SBATCH -p gpu
#SBATCH -e tf_tst.err
#SBATCH -o tf_tst.out
#SBATCH -t 12:00:00
#SBATCH --gres=gpu:1
python ./python_script.py
```

## Submit the batch script to the queue
You can submit the batch job to slurmn and it will be run on a GPU node.
```shell
chmod +x sbatch_script.sh
sbatch sbatch_script.sh
```

## Monitor the job
Check the job is running and on a gpu node:
```shell
squeue -u $USER
```
## Check the output
The output from the print statements in python are diverted to the file `tf_tst.out` and any errors are diverted to `tf_tst.err`. You can check the output with the following command:
```shell
cat tf_tst.out
cat tf_tst.err
```
The physical devices check that there is access to a GPU node. Note if you run the python script directly from the interactive node there will not be a GPU available.

## Test the GPU by training a model
You can now create some more serious TensorFlow code and submit it to the queue to run on a GPU node that will be run on a GPU. You can check the speed on the GPU node compared to the interactive node.  
Create another file `/path/to/your/code/python_train.py` with the following content:

We are using the test training code for CPU vs GPU from this site: [Benchmarking CPU And GPU Performance With Tensorflow](https://www.analyticsvidhya.com/blog/2021/11/benchmarking-cpu-and-gpu-performance-with-tensorflow/)

```python
TODO
```









