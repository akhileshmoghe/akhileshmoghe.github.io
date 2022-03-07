---
title:  "Deep Learning Introduction"
permalink: /_post/ml_ai/deep_learning_introduction
date:   2022-2-21 1:33:22 +0530
categories:
  - Machine Learning
  - Artificial Intelligence
  - Deep Learning
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Machine Learning
  - Artificial Intelligence
  - Deep Learning
author: Akhilesh Moghe
show_author_profile: true
---

## Neural Networks:
- Deep Learning is all about __*<u>Neural Networks</u>*__.
- The structure of a neural network is an interconnected web of __*Nodes/Neurons*__, and the __*Edges*__ that join them together.
- A neural network's main function is to receive a set of inputs, perform progressively complex calculations, and then use the output to solve a problem. Neural networks are used for lots of different applications, like *<u>Classification</u>*.
  - Classification is the process of categorizing a group of objects, while only using some basic data features that describe them.
  - There are lots of classifiers available today - like *<u>Logistic Regression</u>*, *<u>Support Vector Machines</u>*, *<u>Naive Bayes</u>*, and of course, *<u>neural networks</u>*.
- The activation of a classifier, produces a score. For example, say you needed to predict if a patient is sick or healthy, and all you have are their height, weight, and body temperature. The classifier would receive this data about the patient, process it, and fire out a <u>confidence score</u>. A high score would mean a high confidence that the patient is sick, and a low score would suggest that they are healthy.
- Neural nets are used for classification tasks where an object can fall into one of at least two different categories.
- A neural network is highly structured and comes in layers.
  - The first layer is the *<u>input layer</u>*
  - The final layer is the *<u>output layer</u>*
  - and all layers in between are referred to as *<u>hidden layers</u>*.
- A neural net can be viewed as the result of spinning classifiers together in a layered web. This is because *<u>each node in the hidden and output layers has its own classifier</u>*.
- The results of the classification are determined by the scores at each node.
- The first neural nets were born out of the need to address the inaccuracy of an early classifier, the perceptron. It was shown that by using a layered web of perceptrons, the accuracy of predictions could be improved. As a result, this new breed of neural nets was called a __*Multi-Layer Perceptron[MLP]*__.
- __*<u>Forward Propagation</u>*__:
  - This series of events starting from the input where each activation is sent to the next layer, and then the next, all the way to the output, is known as __*<u>forward propagation</u>*__, or forward prop.
  - Forward prop is a neural net's way of classifying a set of inputs.
- __*<u>Weights & Biases</u>*__:
  - Each node has the same classifier, and none of them fire randomly; if you repeat an input, you get the same output.
  - Even if every node in the hidden layer received the same input, each set of inputs is modified by unique weights and biases.
  - For example, the first input is modified by a weight of 10, the second by 5, the third by 6 and then a bias of 9 is added on top.
  - *<u>Each edge has a </u>*__*<u>unique weight</u>*__, *<u>Each node has a </u>*__*<u>unique bias</u>*__. This means that the combination used for each activation is also unique, which explains why the nodes fire differently.
- __*<u>Training Neural Networks</u>*__:
  - The prediction accuracy of a neural net depends on its weights and biases. We want that accuracy to be high, meaning we want the neural net to predict a value that is as close to the actual output as possible, every single time.
  - The process of improving a neural net’s accuracy is called __*training*__, just like with other machine learning methods.
  - To train the net, the output from forward prop is compared to the output that is known to be correct, and the *<u>cost</u>* is the difference of the two. The point of training is to make that cost as small as possible, across millions of training examples. To do this, the net tweaks the weights and biases step by step until the prediction closely matches the correct output.
  - Once trained well, a neural net has the potential to make accurate predictions each time.

