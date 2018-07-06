# CNN with KERAS CIFAR-10

## Image Processing
CIFAR-10 (classification of small images across 10 distinct classes – airplane, automobile, bird, cat, deer, dog, frog, horse, ship & truck).

In last blog I used fully connected Neural Network. However , for images that won't work. CIFAR-10, for example, contains 32×32×3 coloured images: if we are to treat each channel of each pixel as an independent input to an MLP, each neuron of the first hidden layer adds ∼ 3000 new parameters to the model. Also **downsizing** won't help at we will lose info.

Convolutions
It turns out that there is a very efficient way of pulling this off, and it makes advantage of the structure of the information encoded within an image – 
- it is assumed that pixels that are spatially closer together will "cooperate" on forming a particular feature of interest much more than ones on opposite corners of the image. 
- Also, if a particular (smaller) feature is found to be of great importance when defining an image's label, it will be equally important if this feature was found anywhere within the image, regardless of location.

## Convolution operator
- Given a two-dimensional image, I, 
- and a small matrix, K of size h×w, (known as a convolution kernel) which we assume encodes a way of * extracting an interesting image feature* (multiplicatoin of ),
- we compute the convolved image, I∗K, by overlaying the kernel on top of the image in all possible ways, and recording the sum of elementwise products between the image and the kernel:

<img src="https://image.ibb.co/iBVTo8/image.png" alt="image" border="0">
<br>
<br>
**Tip: Convolution is multiplying image with kernel (part of big matrix with small matrix)**
<br >
<img src="https://image.ibb.co/i5pcgT/image.png" alt="image" border="0" width="500px"> 

## Convolutional Layer
The convolution operator forms the fundamental basis of the convolutional layer of a CNN.

- The layer is completely specified by a certain number of kernels, K⃗  (along with additive biases, b⃗ , per each kernel),.
-  it operates by computing the convolution of the output images of a previous layer with each of those kernels, afterwards adding the biases (one per each output image).
-  Finally, an activation function, σ, may be applied to all of the pixels of the output images.
-  typically, the input to a convolutional layer will have d channels (e.g., red/green/blue in the input layer), in which case the kernels are extended to have this number of channels as well,


The final formula of a single output image channel of a convolutional layer (for a kernel K and bias b) as follows::
<img src="https://image.ibb.co/iLmhWT/image.png" alt="image" border="0">


It is important to note that, while a convolutional layer significantly decreases the number of parameters compared to a fully connected (FC) layer, it introduces more hyperparameters – parameters whose values need to be chosen before training starts - 
 - **Depth**: how many different kernels (and biases) will be convolved with the output of the previous layer; 
 - **Height** and width of each kernel; 
 - **Stride**: by how much we shift the kernel in each step to compute the next pixel in the result. This specifies the overlap between individual output pixels, and typically it is set to 1, corresponding to the formula given before. Note that larger strides result in smaller output sizes. 
- **Padding**: note that convolution by any kernel larger than 1×1 will decrease the output image size – it is often desirable to keep sizes the same, in which case the image is sufficiently padded with zeroes at the edges. This is often called "same" padding, as opposed to "valid" (no) padding. It is possible to add arbitrary levels of padding, but typically the padding of choice will be either same or valid.



