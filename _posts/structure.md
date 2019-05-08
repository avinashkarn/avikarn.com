---
layout: post
title: "Structure software: To investigate genetic admixture and overall population structure"
tags: [Structure software, Genetic Admixture, Genetic data, Population structure]
image: /image/structure/
share-img: /image/structure/
---

__Structure Software__ is a freely available software package that one may use for rigorous investigation of `admixed individuals`; identification `point of hybridization` and `migrants`; and over all `structure` of a population using a commonly used genetic markers such as `SNPs` and `SSRs`. This software was developed by __Pritchard Lab__ at __Stanford University__ and can downloaded at this <a href="https://web.stanford.edu/group/pritchardlab/structure.html"> link </a>.

In this tutorial, I will show how to prepare `input` files and run the `Structure` software. For detail information, please read this article at this <a href="https://web.stanford.edu/group/pritchardlab/structure.html">link</a>

<h1> Step 1: Preparing the Input file </h1>
In this tutorial, I am using `numerical` SNP data as in `input` genotype file. One can convert their genotype data in numerical format in `TASSEL` software or any software package available as per ones convenience. The file needs to be foramtted properly as shown below in the image below and save it as `.txt` file.

<center><img src="/image/structure/inputfile" alt="Input File.JPG"></center>

<hr>
__Please Note__ Missing data is denoted as `-9` in the above image. 
<hr>

<h1> Step 2: Importing file in Structure </h1>

<hr>
<center><img src="/image/mds/tassel1.gif" alt="Import data"></center>

<h2> Step: 2 Calculate distance matrix</h2>

<hr>
<center><img src="/image/mds/tassel2.gif" alt="Calculate distance matrix"></center>

<h2> Step: 3 Calculate MDS</h2>

<hr>
<center><img src="/image/mds/tassel3.gif" alt="Calculate MDS"></center>

<h2> Step: 4 Plot PCoAs in TASSEL</h2>

<hr>
<center><img src="/image/mds/tassel4.gif" alt="Plot MDS"></center>

<h2> Optional step: Plotting in R using ggplot2 </h2>
Export the `MDS` results as a `.txt` file and edit in `MS Excel` to add the below `header` and information to plot in `ggplot2`.

<center><img src="/image/mds/mdsfile.JPG" alt="File"></center>

Once the file has bee formatted, One may plot the PCoA results using `ggplot2` library in `R software` using the below commands:

```html
  #Library
  library(ggplot2)

  #import MDS data
  MDS = read.table("MDS.txt", header = T)

  head(MDS)

  #Plot MDS
  MDS_plot <- ggplot(MDS, aes(x=PC1,y=PC2,color=Type, cex=1, label=Sample))
  MDS_plot <- MDS_plot + geom_point() + geom_text(aes(label=Sample),hjust=0, vjust=0)
  MDS_plot
```

<h1> Output </h1>
An example of an ouptut from the `R` command is shown below. In the figure below, the samples represent PCoA of samples from a F1 cross, and one may see that most of the F1 progenies cluster between the parent clusters on the left and right, whereas, there are a few samples (in circle) that cluster with one of the parents indicating that they were a self-pollinated.

<center><img src="/image/mds/plotmds2.png" alt="MDS plot"></center>


<center><h3> --- End of Tutorial --- </h3></center>

__Thank you__ for reading this tutorial. If you have any questions or comments, please let me know in the comment section below or send me an email. 
<form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_top">
<input type="hidden" name="cmd" value="_donations" />
<input type="hidden" name="business" value="8ZF7YRTZ42EKU" />
<input type="hidden" name="item_name" value="To support education for all." />
<input type="hidden" name="currency_code" value="USD" />
<input type="image" src="https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif" border="0" name="submit" title="PayPal - The safer, easier way to pay online!" alt="Donate with PayPal button" />
<img alt="" border="0" src="https://www.paypal.com/en_US/i/scr/pixel.gif" width="1" height="1" />
</form>
Happy QC-ing !


<h3> Bibliography </h3>
<p>Bradbury, Peter J., et al. "TASSEL: software for association mapping of complex traits in diverse samples." Bioinformatics 23.19 (2007): 2633-2635.</p>
<p>Tzeng, Jengnan, Henry Horng-Shing Lu, and Wen-Hsiung Li. "Multidimensional scaling for large genomic data sets." BMC bioinformatics 9.1 (2008): 179. </p>


<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=UA-123359651-1"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'UA-123359651-1');
</script>

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-5126027065024936",
    enable_page_level_ads: true
  });
</script>

