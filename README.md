> Project: "Deep One Class Classification of people"

> Owner: "Tiziana Lasala" 

> Date: "2020:10" 

---

# Deep One Class Classification of people

This repository presents a Deep Learning algorithm able to recognize people instances in pictures.

It is a One-Class Classification (OCC) problem, where objects of a particular class are identified compared to all other possible ones. The person class is called *positive class* or *target class*, while other items are referred to be in the *negative class*, also called *alien class*.
The biggest challenge is represented by the variety of objects opposed to the target class, which does neither allow to model the external class in a univocal way, nor to have all possible cases inside the training set. This problem cannot be solved using traditional techniques of binary and multiclass classifications, precisely because there are no pre-defined classes.

We use **Deep One-class Classification** (DOC), a method proposed by Pramuditha Perera and Vishal M. Patel, targeting OCC problems in computer vision field, like *novelty detection, anomaly detection* and *mobile active authentication*.

The method is described in the article [Learning Deep Features for One-Class Classification](https://arxiv.org/abs/1801.05365), published in November 2019.

## Overview of the project
The key element in the discussion is *learning deep features* that characterize instances of people. The class composed by person examples is highly wide and therefore features are different and heterogeneous, this is very challenging.

Another key element is the concept of *transfer learning* that allows to adapt a labeled dataset from an independent task, called *reference dataset*, to the One-Class Classification, in order to fill the lack coming from alien classes.

A pre-trained Convolutional Neural Network, the high-performance MobileNetV2, is used in combination with two customized loss functions, *compactness loss* and *descriptiveness loss* to realize people recognition.
In this way, specialized features are learned for Deep One-class Classification with two fundamental properties: *compactness*, which means that objects of the same category have similar features located close to each other, and *descriptiveness*, that is elements of different categories have different features placed apart from the rest.

### Training datasets and training framework
There are two training datasets in DOC: the *target dataset* and the *reference dataset*. 

The first one is composed of images of the class we want to recognize: the class person. This dataset should be as heterogeneous as possible, including most of features that can be later isolated as person features. We look for these images in Open Images Dataset V4.

The reference dataset, instead, is an arbitrary set containing unrelated images from multiple classes. In this work, a subset of the training set of ILSVR2012 is used, made of 10000 images from randomly chosen 20 classes. 

The training part is carried out using grayscale images, obtained from the above defined RGB datasets, properly selected and pre-processed. This choice is motivated by a future extension of the Deep One-class Classification in InfraRed images.

A unique MobileNetV2 is instantiated and fed with a big input batch, composed of two smaller batches of the same size: the *target sub-batch* and the *reference sub-batch*.

From the first one, we look for features that characterize the target class coming from *model_features*. The meaningful output is produced by the average pooling layer : 1280 values for each image of the batch, representing the person features we want to impose as close as possible through the *compactness loss* minimization.

From the second one, instead, we are interested in the classification output provided by the fully connected layer: 20 values that rapresent the categorical label of one among the 20 classes from the reference dataset. The *descriptiveness loss* is computed from them and minimized to impose descriptiveness in features.

![losses](https://user-images.githubusercontent.com/62990264/96347251-b4db1300-10a0-11eb-8133-0c77cbaa365e.PNG)

### Testing datasets and testing framework
The testing part is realized by a *template matching framework*: firstly in *template generation phase*, some baseline features of person intances are stored as templates and then, in *matching phase*, a score is generated considering the Euclidean distance between them and new features from the test image. The score is transformed in considerable output for One-Class Classification thanks to a threshold.

![testing](https://user-images.githubusercontent.com/62990264/96347299-0aafbb00-10a1-11eb-9c3c-c0476aefc73d.PNG)

### Achieved results
All models are evaluated using metrics like precision, recall, F1 score, accuracy and the Area Under Curve (AUC) of the ROC curve. Also some graphical tools are employed to make comparisons among resulting methods, like Receiver Operating Characteristic (ROC) curves, Detection Error Tradeoff (DET) curves and the t-distributed Stochastic Neighbor Embedding (t-SNE) visualization of features.

Detailed analysis are performed to understand the impact of changing certain parameters and, later, to find the best model for Deep One-class Classification of people.

In particular, we operate:

* modifying the parameter lambda that weights compactness loss and descriptiveness loss. For examples, we show some images of different loss minimizations:

![lambd](https://user-images.githubusercontent.com/62990264/96347345-7abe4100-10a1-11eb-9029-b2ffb1d0daaf.PNG)

and also pictures with ROC curves of different models:

![lambd1](https://user-images.githubusercontent.com/62990264/96347451-2071b000-10a2-11eb-89cb-9c96cfe4a840.PNG)

* varying the input batch size;

* changing the number of trainable layers and the number of templates;

* decreasing the number of target examples available during the training. 

The best DOC model is the one trained with lambda = 10, input batch size= 32, n. of trainable layers = 40, n. of elements of target dataset= 6000, because it produces similar target features through correct compactness loss minimization and, also, is able to categorize elements of different classes through correct descriptiveness loss minimization. Performances achieved by testing dataset BN1 are very promising:

| AUC        |Precision   | Recall     | F1 score   | Accuracy   |
|:----------:|:----------:|:----------:|:----------:|:----------:|
| 0.997      | 0.989      | 0.976      | 0.982      |   0.983    |

Also, performances remain totally unaffected by the reduction of the number of templates, even when
just one template is present. It follows that our DOC for people recognition succeeds in efficiently
isolating the target class, extracting representative features of a person, as evidenced by the visualization of features through the dimensionality reduction technique *t-SNE*: 

![f40](https://user-images.githubusercontent.com/62990264/96347539-94ac5380-10a2-11eb-94ce-fb97f8216b0e.PNG)

![f5](https://user-images.githubusercontent.com/62990264/96347541-970ead80-10a2-11eb-9b6e-0ea93bb8fb08.PNG)

In the above images, *red points* (with labels 0) are the features associated to images containg people, *green points* (labeled with 1) are the features extracted from pictures with no people and *blue points* (with a fake label 2) are the templates.

In both images the templates are always in the middle of the target region (red), even when their number is decreased. Also, notice that person features are compactly placed together in the red cloud, while features of objects from alien classes are organized into smaller green different
regions far from people: our algorithm has reached the capability to produce features with both compactness and descriptiveness properties.

Finally, Deep One-class Classification is evaluated in contrast to binary one, when the num-
ber of people samples available during the training is reduced: our approach is able to correctly
work when the level of imbalance of the training classes increases, in contrast with the binary
case. In the below figures, ROC curves of DOC (blue) and binary classification (red) models are shown using 600 (first image) and 400 (second image) people samples: the red curve is moving away from the blue one, performing worse than it. 

![ROC600](https://user-images.githubusercontent.com/62990264/96347589-eead1900-10a2-11eb-8780-34cd2343d99a.png)

![ROC400](https://user-images.githubusercontent.com/62990264/96347578-d50bd180-10a2-11eb-9389-5bf1ea1507b0.png)

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

* **trainBN.ipynb**: notebook where training of *DOC* models is performed;

* **imagenet_metadat.txt**: auxiliary file of *trainBN.ipynb*;

* **imagenet_lsvrc_2015_synsets.txt**: auxiliary file of *trainBN.ipynb*;

* **testBN.ipynb**: notebook where testing of *DOC* models is performed on two different grayscale datasets of 2000 elements each;

* **test_surveillance_IR.ipynb**: notebook where testing of *DOC* models is performed on a IR dataset of 110 elements;

* **trainBN_binary.ipynb**: notebook where training of *binary* models is performed to compare them to DOC models;

* **testBN_binary.ipynb**: notebook where testing of *binary* models is performed on two different grayscale datasets of 2000 elements each to compare them to DOC models;

* **testBN_binaryIR.ipynb**: notebook where training of *binary* models is performed on a IR dataset of 110 elements to compare them to DOC models;

* **templatesBN.zip**: folder with examples of 40 grayscale templates;

* **templatesIR.zip**: folder with examples of 20 infrared templates;

* **requirements.txt**: file that contains required libraries.
