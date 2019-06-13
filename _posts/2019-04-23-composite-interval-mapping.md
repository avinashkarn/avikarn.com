---
layout: post
title: "Composite Interval Mapping (CIM) in R software"
tags: [Composite Interval Mapping, QTL, R]
image: /image/cim/scan.png
share-img: /image/cim/scan.png
---

In the __Quantitative Trait Locus (QTL)__ analysis, `composite interval mapping (CIM)` method  estimates the QTL positon with higher accuracy and statistical significance by combining interval mapping with multiple regression. This method also controls background noise resulting from genetic variations in other regions of the genome that affect the detection of the true QTL. In this post, my objective is to describe and walk you through all the basic steps one might need run the composite inteval mapping in `R` software. For further descriptive information on R/qtl, please read the paper by <strong> Karl Broman </strong> at this link: http://www.rqtl.org/
<center>
<iframe src="//rcm-na.amazon-adsystem.com/e/cm?o=1&p=12&l=ur1&category=warehouse&banner=1ER1MDBDMZ0DPV03XWR2&f=ifr&lc=pf4&linkID=55d9573b62d1088d7092fea4c25c0153&t=aviamazon6-20&tracking_id=aviamazon6-20" width="300" height="250" scrolling="no" border="0" marginwidth="0" style="border:none;" frameborder="0"></iframe>
</center>
<center> <h1> To get started</h1> </center>

<h2> Step 1. Download and install R software and R studio </h2>

<ol>
  <li> Download and install the latest version of the <strong> R software </strong> from this
    <a href="https://cran.r-project.org/mirrors.html"> link </a> </li>
  <li> Download and install R studio from this <a href="https://www.rstudio.com/products/rstudio/download">link</a></li>
  <li> Finally, install the library <strong> qtl </strong> in R </li>
</ol>

<h2> Step 1.1 - Install qtl library in R following the below steps: </h2>
<img src="/image/cim/installlibrary.gif">
<hr>

<h2> Step 2. Prepare the input files </h2>

<ol>
  <li> Phenotype data </li>
  <li> Genetic map</li>
</ol>
Both the `phenotype` and `genetic map` infomation can be put together in a single `.csv` file. 
See below the screenshot to understand the formatting of the input file.
<img src="/image/cim/file.png">

<h2> Step 3. Running CIM in R </h2>
Please double check the formatting of the file is correct. Once ready, open `R studio` and type the following commands to get started. 

<h3> Step 3.1. Import <strong>qtl</strong> library </h3>
```html
library(qtl)
```

<h3> Step 3.2. Load the .csv file with the genotype and phenotype data </h3>
```html
phenoGeno <-read.cross(format = "csv", file = "file.csv",  
                 na.strings= c('NA'), genotypes = c("A","B","H"))
```
In the above command, if your genetic data is formatted differently then specify the in `genotype` section. <strong> To execute </strong> the above command, highlight the command and press `CTRL` + `ENTER`. You should see output in the `terminal window`, and please check for any warnings or errors. 

<h3> Step 3.3. Summary of the data file and plotting </h3>
Type:
```html
summary(phenoGeno)
plot(phenoGeno)
```
You should see a plot similiar to shown below:
<img src="/image/cim/summaryplot.png">

<h3> Step 3.4. Calculating genotype probabilities </h3>
In this step, `Hidden Markov model technology` is used to calculate the probabilities of the true underlying genotypes. 
```html
phenoGeno <- calc.genoprob(phenoGeno, step=0, off.end=0.0, error.prob=1.0e-4,stepwidth = "fixed", map.function="kosambi")

phenoGeno_cross <- sim.geno(phenoGeno,  n.draws=32, step=0, off.end=0.0, error.prob=1.0e-4, stepwidth = "fixed", map.function="kosambi")
```
<strong>Important</strong> Make sure to rename `phenoGeno` to `phenoGeno_cross` in the second step of the above command.

<h3> Step 3.5. running QTL scan using CIM method with permutations</h3>
1. Type the below command in the console to run the qtl scan using `CIM` method.
```html
scan.cim = cim(phenoGeno_cross, pheno.col=2, map.function="kosambi")
```
<strong>Note</strong> In `#value` type the column number of the phenotype

2. Run `permutation` test by typing the below command:
```html
scan.cim.perm = cim(phenoGeno_cross, pheno.col=2, map.function="kosambi", n.perm=1000)
```
It is recommended to run at 1,000 permutation test.

3. check the LOD threshold after permutations and significant QTL above the threshold.
```html
summary(scan.cim.perm)
summary(scan.cim, threshold = #value)
```
<strong>Note</strong> In `#value` Add the LOD score at desired `alpha` (0.05 0r 0.1) 

4. Plot the QTL scan
```html
plot(scan.cim)
```
<img src="/image/cim/scan2.png" >

<h3> Step 4.0 QTL effect plot</h3>
Type the below command to plot the `effect plot` of the QTL on the phenotype.
```html
plotPXG(scan_cross, pheno.col = #value, marker = c("#value"))
```
Provide phenotype colum number in `pheno.col` and marker name in `marker =`

Output:
<img src="/image/cim/effect.png">

<h3> Step 5.0 Make QTL model with the significant marker</h3>
```html
qtl <- makeqtl(scan_cross, chr=c(#value), pos=c(#value),what=c("prob")) 
       
fitqtl <- fitqtl(don, pheno.col=c(#value), formula = y~Q1, qtl= qtl, method = "hk", get.ests = T)

summary(fitqtl)
```
<strong>Note</strong> In `#value` Add the signifincat marker's coordinates i.e. `chromosome` and `pos`.

Below is the screenshot of the output one might except to see in the R shell:
```html
fitqtl summary

Method: Haley-Knott regression 
Model:  normal phenotype
Number of observations : 93 

Full model result
----------------------------------  
Model formula: y ~ Q1 

      	df    SS   MS   LOD    %var 	Pvalue(Chi2) Pvalue(F)
Model 	 2  13.07	6.53  30.4  77.8           0         0
Error	 90  3.724093 0.04137881                                        
Total	 92 16.795699 

Estimated effects:
-----------------
               est       SE      t
Intercept  3.01835  0.09467 31.882
9@2.5a    -0.22761  0.09566 -2.379
9@2.5d    -1.41852  0.18935 -7.492

```
<strong>Note: </strong>`Additive effect` (a) = (BB - AA)/2 and `Dominance effect` (d) = AB - (AA + BB)/2.

<h3> Step 6.0 Identify QTL intervals</h3>
```html
lodint(results = scan.cim, chr = #value, drop = 1.8)
```
Provide the `cM` to drop in the `drop =`

<center><h2> --- End of Tutorial --- </h2> </center> 

__Thank you__ for reading this tutorial. I really hope this helpful in giving you the concept and technology behind AmpSeq and the data analysis. If you have any questions or comments, please let comment below or send me an email.
<form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_top">
<input type="hidden" name="cmd" value="_donations" />
<input type="hidden" name="business" value="8ZF7YRTZ42EKU" />
<input type="hidden" name="item_name" value="To support education for all." />
<input type="hidden" name="currency_code" value="USD" />
<input type="image" src="https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif" border="0" name="submit" title="PayPal - The safer, easier way to pay online!" alt="Donate with PayPal button" />
<img alt="" border="0" src="https://www.paypal.com/en_US/i/scr/pixel.gif" width="1" height="1" />
</form>


##  Bibliography
Broman KW, Wu H, Sen Ś, Churchill GA (2003) R/qtl: QTL mapping in experimental crosses. Bioinformatics 19:889-890 
[PubMed | pdf (236k) | Screen pdf (288k)]

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
