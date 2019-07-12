---
layout: post
title:  "Learning the regulatory code of the genome using Basset"
date: 2019-07-12
comments: True
mathjax: True
---
<p>
<h2><b>Introduction</b></h2>
</p>
<p>
Though we know that , most of the diseases are constituted by the non-coding variants, the mechanisms behind these variants are not known. Here, we address this challenge using an approach based on a recent machine learning advance - deep convolutional neural networks (CNNs). We developed a model, Basset to apply CNNs to learn the functional activity of DNA sequences from genomics data.
</p>
<p>
Our model , Basset would be able to:
i. learn the cell-specific regulatory code of the chromatin accessibility
ii. annotate the principles learned by the model.
Thus, Basset offers a powerful computational approach to annotate and interpret the non-coding genome.
</p>
<p>
<h2><b> Overview: </b></h2>
</p>
To learn the DNA sequence signals of open versus closed chromatin in these cells, we apply a deep CNN. CNN's perform adaptive feature extraction to map input data to informative representations during training.
............img
<p>
<h3><b>Convolution layers</b></h3>: The first convolution layer operates directly on the one-hot coding of the input sequence of length 600 bp. It optimizes the weights of set of position weight matrices (PWMs). These PWM filters search for the relevant patterns along the sequence and outputs a matrix with a row for every filter and a column for every position in the sequence. And the subsequent convolution layers consider the orientations and spatial distances between patterns recognized in the previous layer.
</p><p>
We apply a ReLU activation function after each convolution layer to obtain a more expressive model. Because, computing non-linear functions of the information flowing through the network gives more expressive models.
</p><p>
Then we perform the Max Pooling, which reduces the dimension of the input and so the computation in the next layers also decreases. It also provides in-variance to small sequence shifts to the left or right. That is, even if the sequence is slightly shifted to either left or right, it makes the model independent of this behavior.
</p>
<p>
<h3><b>Fully Connected Layers:</b></h3> Fully connected layers perform a linear transformation of the input vector and apply a ReLU.
</p><p>
<h3><b>Prediction layer:</b></h3> The output of the fully connected layers is fed as input to the final layer and the Sigmoid activation is applied.The final layer outputs 164 predictions for the probability that the sequence is accessible in each of the 164 cell types.
</p><p>
The full architecture of our neural network includes three convolution layers and two layers of fully connected hidden nodes.
</p><p>
<h3><b>Implementation:</b></h3>
</h4><b>I. Data Preprocessing:</b></h4>
