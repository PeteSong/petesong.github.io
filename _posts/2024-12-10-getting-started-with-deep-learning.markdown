---
layout: post
title:  "NVIDIA Getting started with Deep Learning"
date:   2024-12-10 15:35:51 +0800
tag: Deep Learning
---

## Basic Concepts


| Humans | Machines  |
| ----- | --- |
| Rest and digest | Training  |
| Fight or flight | Prediction  |


How do children learn?
- Expose them to a large amount of data
- Provide them with the "correct answers"
- They will pick out the important patterns themselves
Deep learning is highly similar to this.

A neural network is essentially a kind of matrix multiplier. What other types of problems require performing a large number of matrix multiplications? Computer graphics! In this science, computer graphic objects are represented by many tiny triangles that work together to present rich visual effects. In animation, simulation, and games, these tiny triangles need to be frequently transformed and rotated, which requires matrix calculations.
Since neural networks and graphics are based on the same fundamental mathematical problems, the hardware for both can be effectively and interchangeably used.
For this kind of training, a particularly important invention is parallel processing. The mathematical operations required for neural network training are not particularly complex, but they must be performed millions, billions, or trillions of times, and it's best to perform these operations simultaneously.
Generally speaking, the CPU you use may have 4 or 8 cores and support parallel processing, but modern GPUs have thousands of cores and are very suitable for deep learning.

Traditional programming: Data + Algorithms = Results
Machine learning: Data + Results = Algorithms

If the decisions needed to obtain the results are very clear, then traditional programming methods should likely be adopted.
However, if the rules have many subtle details or are complex and difficult for humans to describe and even harder to program, then deep learning is a good choice.

This is the relationship of AI/ML/DL/GenAI:
    Artificial Intelligence (Narrow AI)
        Machine Learning
            Deep Learning (Strong AI)
                Generative AI

One of the differences between deep learning and traditional machine learning is the depth and complexity of the networks used. The "depth" here refers to the large number of layers included in the model, which learn the important rules needed to complete a task. These layers are often called "hidden layers"

Deep learning has been applied in:
- Computer vision
    - Robotics and manufacturing
    - Object detection
    - Autonomous driving cars
- Natural language processing
    - Real-time translation
    - Speech recognition
    - Virtual assistants
- Recommendation systems
    - Content planning
    - Targeted advertising
    - Shopping suggestions
- Reinforcement learning
    - Alpha Go defeating the world champion of Go
    - AI robots defeating professional video game players
    - Stock trading robots

## Content

This course includes the following content:
- The "Hello World" program of deep learning
- Training more complex models
- New architectures and techniques to improve performance
- Using pre-trained models
- Transfer learning

## Training a model to understand and classify handwritten digits

Dataset: Use a dataset called MNIST, which contains 70,000 images of handwritten digits
Code: [pytorch_mnist.py](https://github.com/PeteSong/ml-demos/blob/main/ml_demos/pytorch_mnist.py)

In practical operation of machine learning, there are generally the following steps:
- Collect data
- Organize, clean, and enhance the data,
    - Visualize the data
- Select a model
- Train the model with the data
- Evaluate the training effect
- If the effect is not good enough, adjust parameters, data, etc., and re-train
- If the effect is good enough, use this model for prediction, etc.



### A simple linear model

`y = mx + b` ，in the coordinate system, this is a straight line.

Given many (x, y) data, find the corresponding (m, b)
For machine learning, it is necessary to continuously guess (m, b) to find the correct (m, b)
First, randomly select the initial values of (m,b), calculate the estimated value ŷ based on x,
Then, calculate the error, namely MSE and RMSE, that is, the loss function

<div>
$$
MSE=\frac{1}{n} \sum_{i=1}^n(y-y ̂)^2
$$

$$
RMSE=\sqrt{\frac{1}{n} \sum_{i=1}^n(y_i-y ̂_i)^2 }
$$

</div>
Then, the computer needs to adjust (m,b), that is, how to adjust to make the error value smaller and smaller. This is the optimizer.
Here are several terms
- Gradient: Which direction has the most loss reduction
- Learning rate: The distance moved, that is, the distance of adjusting (m,b)
- Training period: Perform a model update using the complete dataset once
- Batch: Some samples in the complete dataset
- Step: One update to the weight parameters
A commonly used optimizer tool is Adm (Adaptive Momentum)


### Building a neural network

An independent learning animation about neural networks: [link](https://developers-dot-devsite-v2-prod.appspot.com/machine-learning/crash-course/backprop-scroll)

For the problem: Identifying the ten handwritten digits from 0 to 9, we use a classification model.
In the MNIST dataset, the size of each picture is 28*28. The pictures are black and white and have only one color channel
Its model is as follows:

    Input layer (the size of the input is 28*28*1) -> Hidden layer -> Output layer (the size of the output is 10)


#### Activation functions
In each neuron, there is an input x, an output y, and an activation function f, y=f(x)
This activation function cannot be linear; otherwise, this model can only learn linear models
Common activation functions
- ReLU
- Sigmoid

The formula of ReLU, when the output is negative, set it to 0
<div>
$$
y ̂=
\begin{cases}
wx+b,  & \text{if wx+b>0} \\
0,     & \text{otherwise}
\end{cases}
$$
</div>
The formula of Sigmoid, it processes the straight line, allowing us to obtain an S-shaped curve between 0 and 1, so that neurons can assign probabilities to the prediction results.
<div>
$$
y ̂= \frac{1}{1 + e^{-(wx+b)}}
$$
</div>

### Training data
We divide the MNIST dataset into two parts: training data and validation data
- Training data: The core dataset used by the model for learning
- Validation data: Used to verify whether the model can truly make understanding (can generalize)
- Overfitting: That is, the model performs well on the training data but poorly on the validation data

### Learning from errors
For this model, we can use `torch.nn.CrossEntropyLoss`, multi-class cross-entropy, a ready-made loss function.
With the loss function, we also need an optimizer to adjust the parameters in the model to make the error smaller and smaller. For this model, we can use `torch.optim.Adam`.


## Training a model to understand and classify American Sign Language

ASL (American Sign Language)

Dataset: [link](https://github.com/PeteSong/ml-demos/tree/main/data/asl_data)
A sign language picture (1*28*28), after conversion, is represented by a one-dimensional array.
Code: [v0](https://github.com/PeteSong/ml-demos/blob/main/ml_demos/pytorch_asl.py), [v1](https://github.com/PeteSong/ml-demos/blob/main/ml_demos/pytorch_asl_cnn_v01.py), [v2](https://github.com/PeteSong/ml-demos/blob/main/ml_demos/pytorch_asl_cnn_v02.py)

### Dataset augmentation
Clean data can provide more preferred examples, and the diversity of the dataset helps the model generalize.
For images, we can randomly perform the following operations to increase the diversity of the dataset([Reference](https://docs.opencv.org/4.x/d9/dab/tutorial_homography.html)).
- Horizontal flip or vertical flip
- Rotate by a certain angle
- Scale the image
- Offset the width and height of the image
- Change the brightness
- Transform the color channel

### Image kernels and convolution
Animation demonstration: https://setosa.io/ev/image-kernels/

As shown in the above dataset, a black and white image can be represented by a matrix, each point is a pixel, 255 represents white, 0 represents black, and the intermediate values represent different grays.
An image kernel is a `3*3` matrix. Multiplying the values of the image kernel with the matrix representing the image can achieve different image processing effects, such as "sharpening", "blurring", etc.
Convolution is multiplying different image kernels with the matrix representing the image and then summing. When a `6*6` image and a `3*3` kernel are convolved, a `4*4` matrix is obtained.

### Convolutional neural networks
Combining image kernels, convolution, and neural networks([History]([https://glassboxmedicine.com/2019/04/13/a-short-history-of-convolutional-neural-networks/](https://glassboxmedicine.com/2019/04/13/a-short-history-of-convolutional-neural-networks/)))，different positions in the image

### Building the model
For the model that recognizes American Sign Language, the structure is as follows:

    Input layer -> Convolutional layer -> Max pooling layer -> Convolutional layer -> Dropout -> Max pooling -> Convolutional layer -> Max pooling -> Dense layer -> Output layer

Different convolutional layers recognize different contents on the image, such as edges, textures, objects, etc.

### Other techniques
#### Max pooling layer
Look at all the values in the window and then take the maximum value and discard the rest. This way, the image will be reduced to a smaller size, thereby reducing the amount of calculation.
#### Dropout
During the training process, we will randomly turn off neurons at a specified dropout rate, which means that the turned-off neurons will not be able to learn in the corresponding training step. This can prevent overfitting and at the same time eliminate excessive dependence on any specific input or neuron.
#### Batch normalization operation
Normalize the amount of weight change between layers during the training process to prevent internal covariate shift

## Pre-trained models and transfer learning
Pre-trained models and transfer learning
Code:
- [vgg16_doggy_door](https://github.com/PeteSong/ml-demos/blob/main/ml_demos/pytorch_vgg16_doggy_door.py)
- [vgg6_presidential_doggy_door](https://github.com/PeteSong/ml-demos/blob/main/ml_demos/pytorch_vgg16_presidential_doggy_door.py)
- [vgg16_fresh_rotten_fruits](https://github.com/PeteSong/ml-demos/blob/main/ml_demos/pytorch_vgg16_fresh_rotten_fruits.py)

### Taking the VGG16 model as an example

The [VGG16](https://arxiv.org/abs/1409.1556) model, namely "Very Deep Convolutional Networks for Large-Scale Image Recognition", won the championship in the 2014 ImageNet Challenge. [ImageNet](http://www.image-net.org/) is a database containing millions of photos that have been labeled into thousands of categories, including animals, food, trees, sports, and humans. Recently, these object detection models perform better than humans, and their accuracy rate exceeds 95%.

### Transfer learning
We take a pre-trained model and cut off its end. Only use the first few layers of the original model as the head layer, and continue to build our own new layers below. This is transfer learning.
Before starting training, we need to freeze the first few layers of the original model that are used. Freezing is a method to prevent the weights from being updated during the training process. This way, only our new layers need to be trained.
#### Fine-tuning
After training, the weights of the original model can be unfrozen and fine-tuned with very small steps to achieve better results.

## Natural language processing
The BERT (Bidirectional Encoder Representations from Transformers) model is used here.
Code: https://github.com/PeteSong/ml-demos/blob/main/ml_demos/pytorch_bert_demo01.py

### Converting words into numbers
Because computers can only handle numbers internally, words need to be converted into numbers. What if we have millions of words? One technique to handle this situation is to generate something called an embedding encoding (also called an embedding vector) for each word. It is called embedding because we map higher-dimensional vectors into (or embed into) a smaller-dimensional space.
Nowadays, people often use transfer learning based on pre-trained models to obtain the embedding vectors of words.


### BERT
We can use the BERT model to predict the next word in a sentence.

### Others

#### 自编码器 (Auto-encoder)
A model whose input and output are exactly the same is an auto-encoder. Cut the auto-encoder in the middle and get an encoder and a decoder.
Its functions are:
- We can use the encoder to convert a large amount of data input into a small amount of data and store it. When the original data is needed, the decoder can be used to restore the original data.
- Anomaly detection. If for some data, their output is significantly different from the input after passing through the auto-encoder, these can be considered as abnormal data.

#### 变分自编码器(Variational auto-encoder)
We can train an auto-encoder with pictures and then use the decoder to generate pictures, that is, Generative AI for Images

#### 扩散模型(Diffusion Model)
Similar to variational autoencoders, but much more complicated, are diffusion models. Diffusion Models work by taking a noisy image and slowly subtracting small amounts of noise at a time until an image is formed. It’s the neural network equivalent of an inkblot test.

Diffusion Models are the bases for many famous image generation tools like Adobe Firefly, Stable Diffusion, and DALL E.

#### 强化学习(Reinforcement learning)
It is a smarter way to use the machine learning models we learned today. Reinforcement learning comes from an idea in psychology called delayed rewards: We can now postpone rewards to obtain better rewards later.
In "reinforcement learning", there are two systems at work: the agent or the intelligent body that tries to learn skills, and the environment that the intelligent body needs to respond to.