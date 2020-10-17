> Project: "Deep One Class Classification of people"

> Owner: "Tiziana Lasala" 

> Date: "2020:10" 

---

# Deep One Class Classification of people

## Description of the project
A Deep Learning algorithm able to recognize people instances in pictures is developed.

This is a One-Class Classification (OCC) problem where objects of a particular class are identified compared to all other possible ones. The person class is called *positive class* or *target class*, while other items are referred to be in the *negative class*, also called *alien class*.
The biggest challenge is represented by the variety of objects opposed to the target class, which does neither allow to model the external class in a univocal way, nor to have all possible cases inside the training set. This problem cannot be solved using traditional techniques of binary and multiclass classifications, precisely because there are no pre-defined classes.

We use **Deep One-class Classification** (DOC), a method proposed by Pramuditha Perera and Vishal M. Patel, targeting One-Class Classification (OCC) problems in computer vision field, like novelty detection, anomaly detection and mobile active authentication.
The method is described in the article [Learning Deep Features for One-Class Classification](https://arxiv.org/abs/1801.05365) published in November 2019.

This approach relies on the concept of transfer learning, since an external multiclass
dataset from an unrelated task, called reference dataset, is employed to
correctly learn deep features characterizing the person class, in addition to the
one-class target dataset

A pre-trained Convolutional Neural Network, the high-performance MobileNetV2,
is used in combination with two loss functions, compactness loss and descriptiveness
loss, that make the variance of features extracted from the target dataset
smaller and minimize the cross-entropy loss of the reference dataset.
In this way, we achieve specialized features for Deep One-class Classification:
they have the two fundamental properties of compactness, which means that
objects of the same category have similar features located close to each other,
and descriptiveness, that is elements of different categories have different features
placed apart from the rest.
The training part is carried out using grayscale images, obtained from RGB pictures
of Open Images Dataset V4 and ILSVRC 2012 dataset, properly selected
and pre-processed.
This choice is motivated by a future extension of the Deep One-class Classification
in InfraRed images, in order to recognize individuals in frames coming
from surveillance videos, even at night.
The testing part is realized by a template matching framework, where, firstly,
some baseline features of person intances are stored and, then, a score is generated
considering the Euclidean distance between new and stored features.
There are three testing datasets, BN1, BN2 and IR, the latter containing InfraRed
pictures, and all models are evaluated using metrics: precision, recall,
F1 score, accuracy and the Area Under Curve (AUC) of the ROC curve.
Also some graphical tools are employed to make comparisons among resulting
methods, like Receiver Operating Characteristic (ROC) curves, Detection Error
Tradeoff (DET) curves and the t-distributed Stochastic Neighbor Embedding (t-
SNE) visualization of features.
The proposed approach is able to achieve very good results in all measurements,
also compared to binary classification algorithms, which require instead a particular
configuration of the training dataset.

## Installation procedure
...

## User Guide
...

P.S. Compile requirements.txt file if needed

More detailed information about markdown style for README.md file [HERE](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

Folder named *data-* must contain folders with target name and synset labels with images inside, as follows:

<pre>
<b>data-</b>
|__ <b>Persona</b>
|__ <b>n01443537</b>
|__ <b>n01532829</b>
|__ <b>n--------</b>
|__ <b>n--------</b>
</pre>

A second folder named *train-* is created to host *target* folder with  people images inside and *reference* folder with images belonging to reference classes inside, as follows:

<pre>
<b>train-</b>
|__ <b>target</b>
    |__ <b>Persona</b>
|__ <b>reference</b>
    |__ <b>African elephant, Loxodonta africana</b>
    |__ <b>acoustic guitar</b>
    |__ <b>analog clock</b>
    |__ <b>backpack, back pack, knapsack, packsack, rucksack, haversack</b>
    |__ <b>beer glass</b>
</pre>
