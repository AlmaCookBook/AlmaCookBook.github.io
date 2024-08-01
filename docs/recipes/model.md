This code is taken and adapted from the website 
[benchmarking-cpu-and-gpu-performance-with-tensorflow](https://www.analyticsvidhya.com/blog/2021/11/benchmarking-cpu-and-gpu-performance-with-tensorflow)  

```python
"""
This code is taken and adapted from the website 
https://www.analyticsvidhya.com/blog/2021/11/benchmarking-cpu-and-gpu-performance-with-tensorflow/
"""
import datetime
def get_now():
    return datetime.datetime.now().strftime('%d-%m-%Y %H:%M')

# 0. Check input is CPU or GPU and time them
import sys
name_of_script = sys.argv[0]
is_cpu = True
if len(sys.argv) > 1:
    if sys.argv[1].lower() == 'gpu':
        is_cpu = False
        
print("Is CPU = ", is_cpu)

starttime = datetime.datetime.now()
print("----------------------------------------------------")
print("Starting at:", get_now())
    

# 1. Import –  necessary modules and the dataset.
import tensorflow as tf
from tensorflow import keras
import numpy as np
import matplotlib.pyplot as plt
print("Loading dataset at",get_now())
(X_train, y_train), (X_test, y_test) = keras.datasets.cifar10.load_data()
print("...loaded at",get_now())

# 2. Perform  Eda – check data and labels shape:
# checking images shape
X_train.shape, X_test.shape
# display single image shape
X_train[0].shape
# checking labels
y_train[:5]

# 3. Apply Preprocessing: Scaling images(NumPy array) by 255 and One-Hot Encoding labels to represent all categories as 0, except  1 for the actual label in ‘float32.’
# scaling image values between 0-1
X_train_scaled = X_train/255
X_test_scaled = X_test/255
# one hot encoding labels
y_train_encoded = keras.utils.to_categorical(y_train, num_classes = 10, dtype = 'float32')
y_test_encoded = keras.utils.to_categorical(y_test, num_classes = 10, dtype = 'float32')

# 4. Model Building: A fn to build a neural network with architecture as below with compiling included :

def get_model():
    model = keras.Sequential([
        keras.layers.Flatten(input_shape=(32,32,3)),
        keras.layers.Dense(3000, activation='relu'),
        keras.layers.Dense(1000, activation='relu'),
        keras.layers.Dense(10, activation='sigmoid')    
    ])
    model.compile(optimizer='SGD',
                loss='categorical_crossentropy',
                metrics=['accuracy'])
    return model


# 5. Training: Train for ten epochs which verbose = 0, meaning no logs.
if is_cpu:
    with tf.device('/CPU:0'):
        print("Running model oin CPUs at",get_now())
        model_cpu = get_model()
        model_cpu.fit(X_train_scaled, y_train_encoded, epochs = 2)
        print("...run at",get_now())
else: 
    with tf.device('/GPU:0'):
        print("Running model on GPUs at",get_now())
        model_gpu = get_model()
        model_gpu.fit(X_train_scaled, y_train_encoded, epochs = 2)
        print("...run at",get_now())
        
endtime = datetime.datetime.now()
diff = endtime - starttime
print('Job took: ', diff.days, diff.seconds, diff.microseconds)
```