---
layout: post
title:  "Non Coding Variant Effect Prediction using Basenji"
date: 2019-07-02
comments: True
mathjax: True
---
<h2> <b> Introduction </b> </h2>
Human DNA is about 99.5% identical from person to person. However, there are small differences that make each person unique. These differences are called variants. Some of these genetic variants are common, such as the variants for blood types (A, B, AB, or O), while many of the variants are rare, seen in only a few people. And a few among these variants would be associated with the disease trait. And by predicting these disease-associated variants, we would get to know what exactly is driving the disease and could get the right treatment for suppressing the disease.
<p>
Variants are of two types, Coding and Non-coding. 
<p>
<b>Coding DNA:</b> The DNA which encodes the proteins are called coding DNA.These Coding DNA is responsible for transcription and translation.
  </p><p>
<b>Non Coding DNA:</b> DNA which do not encode proteins are called Non-coding DNA.But they perform very important functions which includes regulating transcription and translation, producing different types of RNA, such as microRNA, and protecting the ends of chromosomes.</p>
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
<h4><b> Distal regulatory elements:</b></h4></p>
<p>
We devised a method to quantify how distal sequence influences a Basenji model's predictions and applied it to produce saliency maps for gene regions. </p><p>
  <b>Saliency score= ∑ 128-bp bin representations * Gradient of the model predictions</b></p>
  <p>{% include image.html align="center" url="/assets/img/peaks.jpg" %} </p>
  <p>
  Peaks in this saliency score detect distal regulatory elements, and its sign indicates enhancing (+) versus silencing (−) influence.
 </p>
 <p>
<h4><b> c.Prediction Layer:</b></h4></p>
<p>Finally, we apply a final width-one convolutional layer to parameterize a multitask Poisson regression on normalized counts of aligned reads to that region for every data set provided and it predicts the 4229 coverage datasets.</p><p>
<h3><b>Output:</b></h3></p><p>
The model's ultimate goal is to predict read coverage in 128-bp bins across long chromosome sequences which would be then used to predict the regulatory activity function.</p>
<p>
<h3><b>Usage of these predicted coverage values:</b></h3></p>
<p>
Coverage tells the number of unique reads that include a given nucleotide in the reconstructed sequence.
Given a SNP–gene pair, we define its SNP expression difference (SED) score as the difference between the predicted CAGE coverage at that gene's TSS (or summed across multiple alternative TSS) for the two alleles.</p><p>
  <b><center>SED= | Allele1 - Allele2|</center></b></p>
 <p>{% include image.html align="center" url="/assets/img/saliency maps.jpg" %}</p>
 <p>By considering this |SED - LD| score, all the variants are ranked. And this gives the information about the causal variants i.e, based on the rank of the variants. And then, these variants can be linked to disease loci and we can thus know the genetic basis of the disease.</p>

