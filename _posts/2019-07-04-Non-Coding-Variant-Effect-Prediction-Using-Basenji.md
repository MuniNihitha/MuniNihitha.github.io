---
layout: post
title:  "Non Coding Variant Effect Prediction using Basenji"
date: 2019-07-02
comments: True
mathjax: True
---
<h2> <b> Introduction </b> </h2>
Human DNA is about 99.5% identical from person to person. However, there are small differences that make each person unique. These differences are called variants. And these variants are linked to certain health conditions.
<p>
Variants are of two types, Coding and Non-coding. 
<p>
<b>Coding DNA:</b> The DNA which encodes the proteins are called coding DNA.
  </p><p>
<b>Non Coding DNA:</b> DNA which do not encode proteins are called Non-coding DNA.</p>
<p>
In order to understand the genetic basis of disease, we need to understand the impact of variants across the genome.</p>
<p>
 </p>
<h3><b>Why Non Coding Variants?</b></h3>
<p>
  Only about 1 percent of DNA is made up of protein-coding genes; the other 99 percent is non coding. Scientists once thought non coding DNA was "junk," with no known purpose. However, it is becoming clear that at least some of it is integral to the function of cells, particularly the control of gene activity and majority of the diseases are caused by these non-coding variants.
  </p>
  <p>
<h3><b>Predicting the effects of non-coding variants</b></h3></p>
<p>
  We need to build models that can predict molecular phenotypes directly from biological sequences to probe the association between the phenotype and the genetic variation.</p>
  <p>
Thus, sequence based deep learning models can be used for assessing the impact of such variants which offer a promising approach to find potential drivers of complex phenotypes.
  </p>
  <p>
  In this blog, we will learn about one such sequence based deep learning model,<b> Basenji </b>.</p>
 <p>
<h2> <b> Basenji </b></h2>
</p>
<p>
Models for predicting phenotypic outcomes from genotypes have important applications to understanding genomic function and improving human health. Basenji is a machine-learning system to predict cell-type–specific epigenetic and transcriptional profiles in large mammalian genomes from DNA sequence alone.
</p>
<p>
  Numerous lines of evidence suggest that many non coding variants influence traits by changing gene expression. Thus, gene expression offers a tractable intermediate phenotype for which improved modeling would have great value.
  </p>
  <p>
  </p>
  <h3><b>Model Architecture </b></h3>
  <p>
   {% include image.html align="center" url="/assets/img/basenjifull.jpg" %}
  </p>
  <p>
<h3><b>Input:</b></h3>
The model accepts much larger (2^17=) 131-kb regions as input i.e, the entire DNA sequence. DNA sequences come in to the model one hot encoded to four rows representing A, C, G, and T.
</p>
<p>
<h3><b>Architecture:</b></h3>
</p><p>
Basenji is basically a deep convolutional neural network with three layers i.e, Convolution, Dilated convolution and Prediction layers. Lets discuss these layers in detail.</p><p>
<h4><b>a.Convolution layers:</b></h4></p>
<p>
It performs multiple layers of convolution and pooling to transform the DNA to a sequence of vectors representing 128-bp regions. We used a Basenji architecture with four standard convolution layers, pooling in between layers by two, four, four, and four to a multiplicative total of 128.
</p><p>
<h4><b>b.Dilated Convolution layers:</b></h4>
</p>
To share information across long distances, we then apply several layers of densely connected dilated convolutions. After these layers, each 128-bp region aligns to a vector that considers the relevant regulatory elements across a large span of sequence. Dilated convolutions extend the reach of our model to view distal regulatory elements at distances, achieving a 32-kb receptive field width. Lets discuss below how to predict the effect of distal regulatory elements in detail.<p>
  </p>
<p>
  {% include image.html align="center" url="/assets/img/dilatedconv.jpg" %}
 </p>
<p>
<h3><b> Distal regulatory elements:</b></h3></p>
<p>
We devised a method to quantify how distal sequence influences a Basenji model's predictions and applied it to produce saliency maps for gene regions. </p><p>
  <b>Saliency score= ∑ 128-bp bin representations * Gradient of the model predictions</b></p>
  <p>{% include image.html align="center" url="/assets/img/saliency maps.jpg" %} </p>
  <p>
  Peaks in this saliency score detect distal regulatory elements, and its sign indicates enhancing (+) versus silencing (−) influence.
 </p>
