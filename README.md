> Project: "Deep One Class Classification of people"

> Owner: "Tiziana Lasala" 

> Date: "2020:10" 

---

# Deep One Class Classification of people

This repository presents a Deep Learning algorithm able to recognize people instances in pictures.

TIt is a One-Class Classification (OCC) problem where objects of a particular class are identified compared to all other possible ones. The person class is called *positive class* or *target class*, while other items are referred to be in the *negative class*, also called *alien class*.
The biggest challenge is represented by the variety of objects opposed to the target class, which does neither allow to model the external class in a univocal way, nor to have all possible cases inside the training set. This problem cannot be solved using traditional techniques of binary and multiclass classifications, precisely because there are no pre-defined classes.

We use **Deep One-class Classification** (DOC), a method proposed by Pramuditha Perera and Vishal M. Patel, targeting OCC problems in computer vision field, like *novelty detection, anomaly detection* and *mobile active authentication*.

The method is described in the article [Learning Deep Features for One-Class Classification](https://arxiv.org/abs/1801.05365), published in November 2019.

## Overview of the project
The key element in the discussion is *learning deep features* that characterize instances of people. The class composed by person examples is highly wide and therefore features are different and heterogeneous, this is very challenging.

Another key element is the concept of *transfer learning* that allows to adapt a labeled dataset from an independent task, called *reference dataset*, to the One-Class Classification, in order to fill the lack coming from alien classes.

A pre-trained Convolutional Neural Network, the high-performance MobileNetV2, is used in combination with two customized loss functions, *compactness loss* and *descriptiveness loss* to realize people recognition.
Specialized features for Deep One-class Classification have the two fundamental properties of *compactness*, which means that objects of the same category have similar features located close to each other, and *descriptiveness*, that is elements of different categories have different features placed apart from the rest.

### Training datasets and training framework
There are two training datasets in DOC: the *target dataset* and the *reference dataset*. 

The first one is composed of images of the class we want to recognize: the class person. This dataset should be as heterogeneous as possible, including most of features that can be later isolated as person features. We look for these images in Open Images Dataset V4.

The reference dataset, instead, is an arbitrary set containing unrelated images from multiple classes. In this work, a subset of the training set of ILSVR2012 is used, made of 10000 images from randomly chosen 20 classes. 

The training part is carried out using grayscale images, obtained from the above defined RGB datasets, properly selected and pre-processed. This choice is motivated by a future extension of the Deep One-class Classification in InfraRed images.

A unique MobileNetV2 is instantiated and fed with a big input batch, composed of two smaller batches of the same size: the *target sub-batch* and the *reference sub-batch*. From the first one, we look for features that characterize the target class coming from *model_features*. The meaningful output is produced by the average pooling layer : 1280 values for each image of the batch, representing the person features we want to impose as close as possible through the *compactness loss* minimization.

From the second one, instead, we are interested in the classification output provided by the fully connected layer: 20 values that rapresent the categorical label of one among the 20 classes from the reference dataset. The *descriptiveness loss* is computed from them and minimized to impose descriptiveness in features.

![losses](https://user-images.githubusercontent.com/62990264/96336455-4423fe80-1080-11eb-8561-ca150142705c.PNG)

### Testing datasets and testing framework
The testing part is realized by a *template matching framework*: firstly, some baseline features of person intances are stored as templates and then, in *matching phase*, a score is generated considering the Euclidean distance between them and new features from the test image. The score is transformed in considerable output for One-Class Classification thanks to a threshold.

### Achieved results
All models are evaluated using metrics like precision, recall, F1 score, accuracy and the Area Under Curve (AUC) of the ROC curve.
Also some graphical tools are employed to make comparisons among resulting
methods, like Receiver Operating Characteristic (ROC) curves, Detection Error
Tradeoff (DET) curves and the t-distributed Stochastic Neighbor Embedding (t-
SNE) visualization of features.
The proposed approach is able to achieve very good results in all measurements, also compared to binary classification algorithms, which require instead a particular configuration of the training dataset.

## Current environment
* Ubuntu 18.04.5 supplied by GPU NVIDIA GeForce RTX 2080 with 8GB of memory
* Python 3.6.9

## Python libraries used
* numpy 1.19.0
* sklearn 0.23.1
* matplotlib 3.2.2
* OpenCV-python 4.3.0.36
* Tensorflow-gpu 2.2.0 
* Keras 2.4.3

## User Guide
Following files are present in the repository:


...

P.S. Compile requirements.txt file if needed

