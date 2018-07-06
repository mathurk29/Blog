# (Artificial) Neurons
  While the term "deep learning" allows for a broader interpretation, in pratice, for a vast majority of cases, it is applied to the model of (artificial) neural networks. These biologically inspired structures attempt to mimic the way in which the neurons in the brain process percepts from the environment and drive decision-making. In fact, a single artificial neuron (sometimes also called a perceptron) has a very simple mode of operation – it computes a weighted sum of all of its inputs $$x⃗$$ , using a weight vector w⃗  (along with an additive bias term, w0), and then potentially applies an activation function, σ, to the result.

## Perceptron

<img src = "https://cambridgespark.com/content/tutorials/deep-learning-for-complete-beginners-recognising-handwritten-digits/figures/neuron.jpeg">

Some of the popular choices for activation functions include (plots given below):

identity: σ(z)=z;
sigmoid: especially the logistic function, σ(z)=11+exp(−z), and the hyperbolic tangent, σ(z)=tanhz;
rectified linear (ReLU): σ(z)=max(0,z).

## Enter Neural networks (& Deep Learning)

A neuron is completely specified by its weight vector w⃗ , and the key aim of a learning algorithm is to assign appropriate weights to the neuron based on a training set of known input/output pairs, such that the notion of a "predictive error/loss" will be minimised when applying the inputs within the training set to the neuron. One typical example of such a learning algorithm is gradient descent, which will, for a particular loss function E(w⃗ ), update the weight vector in the direction of steepest descent of the loss function, scaled by a learning rate parameter η:

w⃗ ←w⃗ −η∂E(w⃗ )∂w⃗

The loss function represents our belief of how "incorrect" the neuron is at making predictions under its current parameter values.


Once we have a notion of a neuron, it is possible to connect outputs of neurons to inputs of other neurons, giving rise to neural networks. In general we will focus on feedforward neural networks, where these neurons are typically organised in layers, such that the neurons within a single layer process the outputs of the previous layer. The most potent of such architectures (a multilayer perceptron or MLP) fully connects all outputs of a layer to all the neurons in the following layer, as illustrated below.

<img src = "https://cambridgespark.com/content/tutorials/deep-learning-for-complete-beginners-recognising-handwritten-digits/figures/mlp.png">


Any neural network with more than one hidden layer is considered deep.

## Steps for DL on MNIST:

1. Import 
```python
from keras.datasets import mnist # subroutines for fetching the MNIST dataset
from keras.models import Model # basic class for specifying and training a neural network
from keras.layers import Input, Dense # the two types of neural network layer we will be using
from keras.utils import np_utils # utilities for one-hot encoding of ground truth values
```
2. We'll define some parameters of our model. These are often called hyperparameters, because they are assumed to be fixed before training starts:
    The batch size, representing the number of training examples being used simultaneously during a single iteration of the gradient descent algorithm.
    The number of epochs, representing the number of times the training algorithm will iterate over the entire training set before terminating.
    The number of neurons in each of the two hidden layers of the MLP.
```python
batch_size = 128 # in each iteration, we consider 128 training examples at once
num_epochs = 20 # we iterate twenty times over the entire training set
hidden_size = 512 # there will be 512 neurons in both hidden layer
```

3. To preprocess the input data, we will first flatten the images into 1D (as we will consider each pixel as a separate input feature), and we will then force the pixel intensity values to be in the [0,1] range by dividing them by 255. This is a very simple way to "normalise" the data, and I will be discussing other ways in future tutorials in this series.

A good approach to a classification problem is to use probabilistic classification, i.e. to have a single output neuron for each class, **outputting a value which corresponds to the probability of the input being of that particular class** 
This implies a need to transform the training output data into a "one-hot" encoding: for example, if the desired output class is 3, and there are five classes overall (labelled 0 to 4), then an appropriate one-hot encoding is: [0 0 0 1 0]. Keras, once again, provides us with an out-of-the-box functionality for doing just that.
  
```Python
num_train = 60000 # there are 60000 training examples in MNIST
num_test = 10000 # there are 10000 test examples in MNIST

height, width, depth = 28, 28, 1 # MNIST images are 28x28 and greyscale
num_classes = 10 # there are 10 classes (1 per digit)

(X_train, y_train), (X_test, y_test) = mnist.load_data() # fetch MNIST data

X_train = X_train.reshape(num_train, height * width) # Flatten data to 1D
X_test = X_test.reshape(num_test, height * width) # Flatten data to 1D
X_train = X_train.astype('float32') 
X_test = X_test.astype('float32')
X_train /= 255 # Normalise data to [0, 1] range
X_test /= 255 # Normalise data to [0, 1] range

Y_train = np_utils.to_categorical(y_train, num_classes) # One-hot encode the labels
Y_test = np_utils.to_categorical(y_test, num_classes) # One-hot encode the labels
```

Now is the time to actually define our model! 

To do this we will be using a stack of three Dense layers, which correspond to a fully unrestricted MLP structure, linking all of the outputs of the previous layer to the inputs of the next one. The dense layer is fully connected layer, so all the neurons in a layer are connected to those in a next layer. The dropout drops connections of neurons from the dense layer to prevent overfitting. A dense layer is a classic fully connected neural network layer : each input node is connected to each output node.

We will use ReLU activations for the neurons in the first two layers, and a softmax activation for the neurons in the final one.

### Define all layers and identify the input/output layer

```Python
inp = Input(shape=(height * width,)) # Our input is a 1D vector of size 784
hidden_1 = Dense(hidden_size, activation='relu')(inp) # First hidden ReLU layer
hidden_2 = Dense(hidden_size, activation='relu')(hidden_1) # Second hidden ReLU layer
out = Dense(num_classes, activation='softmax')(hidden_2) # Output softmax layer

model = Model(inputs=inp, outputs=out) # To define a model, just specify its input and output layers
```

## Cross Entropy

For a particular output probability vector y⃗ , compared with our (ground truth) one-hot vector y^⃗ , the loss (for k-class classification) is defined by

<img src="https://image.ibb.co/dHdUAo/Capture.png" alt="Capture" border="0">

This loss is better for probabilistic tasks (i.e. ones with logistic/softmax output neurons), primarily because of its manner of derivation – it aims only to maximise the model's confidence in the correct class, and is not concerned with the distribution of probabilities for other classes (while the squared error loss would dedicate equal attention to getting all of the other class probabilities as close to zero as possible). This is due to the fact that incorrect classes, i.e. classes i′ with y^i′=0, eliminate the respective neuron's output from the loss function.

```
model.compile(loss='categorical_crossentropy', # using the cross-entropy loss function
              optimizer='adam', # using the Adam optimiser
              metrics=['accuracy']) # reporting the accuracy
              
model.fit(X_train, Y_train, # Train the model using the training set...
          batch_size=batch_size, epochs=num_epochs,
          verbose=1, validation_split=0.1) # ...holding out 10% of the data for validation
          
model.evaluate(X_test, Y_test, verbose=1) # Evaluate the trained model on the test set!
```





## Just show me the code!

```Python
from keras.datasets import mnist # subroutines for fetching the MNIST dataset
from keras.models import Model # basic class for specifying and training a neural network
from keras.layers import Input, Dense # the two types of neural network layer we will be using
from keras.utils import np_utils # utilities for one-hot encoding of ground truth values

batch_size = 128 # in each iteration, we consider 128 training examples at once
num_epochs = 20 # we iterate twenty times over the entire training set
hidden_size = 512 # there will be 512 neurons in both hidden layers

num_train = 60000 # there are 60000 training examples in MNIST
num_test = 10000 # there are 10000 test examples in MNIST

height, width, depth = 28, 28, 1 # MNIST images are 28x28 and greyscale
num_classes = 10 # there are 10 classes (1 per digit)

(X_train, y_train), (X_test, y_test) = mnist.load_data() # fetch MNIST data

X_train = X_train.reshape(num_train, height * width) # Flatten data to 1D
X_test = X_test.reshape(num_test, height * width) # Flatten data to 1D
X_train = X_train.astype('float32') 
X_test = X_test.astype('float32')
X_train /= 255 # Normalise data to [0, 1] range
X_test /= 255 # Normalise data to [0, 1] range

Y_train = np_utils.to_categorical(y_train, num_classes) # One-hot encode the labels
Y_test = np_utils.to_categorical(y_test, num_classes) # One-hot encode the labels

inp = Input(shape=(height * width,)) # Our input is a 1D vector of size 784
hidden_1 = Dense(hidden_size, activation='relu')(inp) # First hidden ReLU layer
hidden_2 = Dense(hidden_size, activation='relu')(hidden_1) # Second hidden ReLU layer
out = Dense(num_classes, activation='softmax')(hidden_2) # Output softmax layer

model = Model(inputs=inp, outputs=out) # To define a model, just specify its input and output layers

model.compile(loss='categorical_crossentropy', # using the cross-entropy loss function
              optimizer='adam', # using the Adam optimiser
              metrics=['accuracy']) # reporting the accuracy

model.fit(X_train, Y_train, # Train the model using the training set...
          batch_size=batch_size, epochs=num_epochs,
          verbose=1, validation_split=0.1) # ...holding out 10% of the data for validation
model.evaluate(X_test, Y_test, verbose=1) # Evaluate the trained model on the test set!
```

