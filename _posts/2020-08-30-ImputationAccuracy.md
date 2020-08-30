---
layout: post
title: "Evaluate Genotype Accuarcy in TASSEL GUI"
tags: [Imputation, GUI, Genotype, TASSEL, Bioinformatics]
image: /image/imputeAccuracy/plotImputeAccuracy.png
share-img: /image/imputeAccuracy/plotImputeAccuracy.png
---

Compare imputation accuracies of different imputation methods (Beagle 5.1, LinkImputeR, kNNi, FILLIN) by calculating the percentage of correctly imputed genotypes in TASSEL GUI software. Please find more information on `TASSEL` software and its documentaton at this <a href="https://www.maizegenetics.net/tassel"> Link </a>

<br>

In this tutorial, I am only showing how one can evaluate the imputation accuracy in TASSEL GUI using `LinkImpute (LD kNNi) imputation algorithm. In my research experince, I have worked with genotype data of maize, teosinte, soybean, and grapes, and <a href="https://www.g3journal.org/content/5/11/2383.short">LinkImpute</a> has been an efficient imputation algorithm with low imputation error rate. However, I strongly suggest to try other imputation methods and compare their error rates, which can be easily done in TASSEL GUI software by following the below steps:

- `Import` your genotype data (VCF, Hapmap and other formats)
- `Mask` about 1-10% of the genotype data using `Mask Genotype` plugin under `Data`
- `Impute` the masked genotype data (I use LD kNNi or linkimpute) or load imputed data using different platform such as Beagle, but <strong> make sure that imputation was performed on the SAME masked genotype data </strong>
- `Select` the three files (Raw data, Masked data, and Imputed), and click `Evaluate Imputation Accuracy` under `Impute` and press OK
- Summary of the evaluation should be generated in the new node

<h2> Steps in TASSEL GUI</h2>
The above steps are also shown in the below steps:
<img src="/image/imputeAccuracy/imputationAccuracy_1.gif">

<br>
<h2> Plot imputation accuracies </h2>
One can plot the imputation accuracies by exporting the evaluation summary stats in R or Excel as shown in example below:
<img src="/image/imputeAccuracy/plotImputeAccuracy.png">

	
__Thank you__ for reading this tutorial. I really hope these steps will assist in your analysis. If you have any questions or comments, please comment below or send an email. 

<h3> Bibliography </h3>
Glaubitz, J. C., Casstevens, T. M., Lu, F., Harriman, J., Elshire, R. J., Sun, Q., & Buckler, E. S. (2014). TASSEL-GBS: a high capacity genotyping by sequencing analysis pipeline. PloS one, 9(2), e90346.

Money, Daniel, et al. "LinkImpute: fast and accurate genotype imputation for nonmodel organisms." G3: Genes, Genomes, Genetics 5.11 (2015): 2383-2390.
