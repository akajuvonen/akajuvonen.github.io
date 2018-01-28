---
layout: post
title:  "Hand-written Digit Recognition with Deep Learning"
---

Intro.

# Data description

The [MNIST][mnist] database consists of images of handwritten digits. The
dataset has 60,000 samples for training and 10,000 for testing. The images
contain normalized and centered samples with 28x28 pixels. Using MNIST database
for handwritten digit recognition is a very typical example case, often using
[convolutional neural networks][cnn] (CNN).

# Version 1

Imports:

```python
from keras.utils import to_categorical
from keras.datasets import mnist
from keras.models import Sequential
from keras.layers import Conv2D, MaxPooling2D, Dense, Flatten
from keras.losses import categorical_crossentropy
from keras.optimizers import Adam
from sklearn.metrics import confusion_matrix
import numpy as np
```

Data loading and preprocessing:

```python
# Load data, training and testing sets
# The data is already shuffled at this point
(x_train, y_train), (x_test, y_test) = mnist.load_data()

# Reshape data to include channel dimension, required for Keras
x_train = x_train.reshape(x_train.shape[0], 28, 28, 1)
x_test = x_test.reshape(x_test.shape[0], 28, 28, 1)

# Convert to float32
x_train = x_train.astype('float32')
x_test = x_test.astype('float32')

# Scale the values between 0 and 1
x_train = x_train / 255
x_test = x_test / 255

# Perform one-hot encoding for output
y_train = to_categorical(y_train, N_CLASSES)
y_test = to_categorical(y_test, N_CLASSES)
```

First neural net version (simple). A minimal CNN with one 2D convolutional
layer, max pooling layer and a normal densely connected neural network layer
with 10 outputs (for 10 different digits).

```python
# We use the sequential model
model = Sequential()
# A 2-D convolutional layer
model.add(Conv2D(filters=16, kernel_size=(3, 3), activation='relu',
                 input_shape=INPUT_SHAPE))
# Pooling layer with pool size and strides = (2,2)
model.add(MaxPooling2D(pool_size=2))
# Flattens the output for normal neural network layer
model.add(Flatten())
# Output layer with 10 outputs, uses softmax classifier
model.add(Dense(10, activation='softmax'))
```

Compile and fit. We have to compile the model, first. In this case Adam is
used for optimization.

```python
# Compile the model with Adam optimizer
model.compile(optimizer=Adam(), loss=categorical_crossentropy,
              metrics=['accuracy'])

# Fit the model
model.fit(x_train, y_train, batch_size=BATCH_SIZE, epochs=EPOCHS,
          validation_data=(x_test, y_test))
```

Results and metrics. Print the final loss (try minimize) and accuracy (try to
maximize). Also a confusion matrix.

```python
# Print the final metrics
loss, accuracy = model.evaluate(x_test, y_test, verbose=0)
print("Loss: ", loss)
print("Accuracy: ", accuracy)

# Print confusion matrix
y_predicted = model.predict(x_test)
y_predicted = np.argmax(y_predicted, axis=1)
y_actual = np.argmax(y_test, axis=1)
cm = confusion_matrix(y_actual, y_predicted)
print(cm)
```

Results below:

```
Loss:  0.0576041658529
Accuracy:  0.9819
[[ 970    0    1    1    0    1    4    1    2    0]
 [   0 1131    1    1    1    0    0    0    1    0]
 [   4    7 1003    3    1    0    3    5    5    1]
 [   1    0    1  993    0    7    0    2    4    2]
 [   2    1    0    0  965    0    5    0    1    8]
 [   1    0    1    4    0  883    2    0    1    0]
 [   7    3    1    0    2    1  943    0    1    0]
 [   0    4    9    3    2    0    0 1004    2    4]
 [   5    0    2    1    1    2    1    2  958    2]
 [   2    3    0    2   14    5    0    8    6  969]]
```

# Version 2

More layers:

```python
# We use the sequential model
model = Sequential()
# A 2-D convolutional layer
model.add(Conv2D(filters=16, kernel_size=(3, 3), activation='relu',
                 input_shape=INPUT_SHAPE))
model.add(Conv2D(filters=16, kernel_size=(3, 3), activation='relu'))
# Pooling layer with pool size and strides = (2,2)
model.add(MaxPooling2D(pool_size=2))
model.add(Conv2D(filters=32, kernel_size=(3, 3), activation='relu'))
model.add(Conv2D(filters=32, kernel_size=(3, 3), activation='relu'))
model.add(MaxPooling2D(pool_size=2))
# Flattens the output for normal neural network layer
model.add(Flatten())
model.add(Dense(128, activation='relu'))
# Output layer with 10 outputs, uses softmax classifier
model.add(Dense(10, activation='softmax'))
```

Results:

```
Loss:  0.034049704785
Accuracy:  0.9912
[[ 978    0    0    0    0    0    0    1    1    0]
 [   1 1130    0    1    0    0    1    2    0    0]
 [   1    0 1021    0    0    0    0    8    2    0]
 [   0    0    2 1004    0    2    0    0    2    0]
 [   0    0    0    0  976    0    1    0    0    5]
 [   1    0    0    6    0  883    2    0    0    0]
 [   3    2    1    0    2    5  942    0    3    0]
 [   0    3    1    1    0    0    0 1018    1    4]
 [   3    0    1    0    1    0    1    1  962    5]
 [   0    0    0    0    5    6    0    0    0  998]]
```

# Version 3

Added dropout.

```python
from keras.layers import Dropout
```

```python
# We use the sequential model
model = Sequential()
# A 2-D convolutional layer
model.add(Conv2D(filters=16, kernel_size=(3, 3), activation='relu',
                 input_shape=INPUT_SHAPE))
model.add(Conv2D(filters=16, kernel_size=(3, 3), activation='relu'))
# Pooling layer with pool size and strides = (2,2)
model.add(MaxPooling2D(pool_size=2))
# Add a dropout layer to avoid overfitting
model.add(Dropout(0.25))
model.add(Conv2D(filters=32, kernel_size=(3, 3), activation='relu'))
model.add(Conv2D(filters=32, kernel_size=(3, 3), activation='relu'))
model.add(MaxPooling2D(pool_size=2))
model.add(Dropout(0.25))
# Flattens the output for normal neural network layer
model.add(Flatten())
model.add(Dense(128, activation='relu'))
model.add(Dropout(0.25))
# Output layer with 10 outputs, uses softmax classifier
model.add(Dense(10, activation='softmax'))
```

Results:

```
Loss:  0.0138718557994
Accuracy:  0.9959
[[ 978    0    0    0    0    0    0    1    1    0]
 [   0 1132    0    1    0    0    1    1    0    0]
 [   0    1 1028    0    0    0    1    2    0    0]
 [   0    0    0 1007    0    3    0    0    0    0]
 [   0    0    0    0  980    0    0    0    0    2]
 [   0    0    0    5    0  886    1    0    0    0]
 [   2    2    0    0    1    1  952    0    0    0]
 [   0    2    0    0    0    0    0 1025    0    1]
 [   0    0    1    1    0    0    0    0  970    2]
 [   1    0    0    1    2    3    0    1    0 1001]]
```

[mnist]: http://yann.lecun.com/exdb/mnist/
[cnn]: https://en.wikipedia.org/wiki/Convolutional_neural_network
