---
layout: post
title: "How to construct a genetic map?"
tags: [Lep-MAP3, Genetic Mapping, Mapchart]
image: /image/lepmap/cropped_nih_geneticmap.jpg
share-img: /image/lepmap/cropped_nih_geneticmap.jpg
---

__Building genetic maps__ can be challenging and sometimes quite stressful, especially, when dealing with thousands or even millions of markers. In this post, I am hoping to help anyone who would like to get started to build a decent genetic map in an open software 
<a href="https://sourceforge.net/projects/lep-map3/"> Lep-MAP3 </a>, and finally, the evaluating the accuarcy of the map and plotting it.

The steps invloved in the genetic mapping process in Lep-MAP3 are shown in the flow chart below. 

<center><img src="/image/lepmap/lepmap_flow.png"></center>
<a href="https://click.linksynergy.com/fs-bin/click?id=876kEArXFCo&offerid=551783.18&subid=0&type=4" rel="nofollow"><IMG border="0"   alt="Qatar Airways" src="https://ad.linksynergy.com/fs-bin/show?id=876kEArXFCo&bids=551783.18&subid=0&type=4&gridnum=16"></a>
<hr>


<h2> Running Lep-MAP3 </h2>

<h3>Step 1.1. Installation and File Preparation</h3>
<strong> Important - </strong> Correctly install the Lep-MAP3 software on your computer, and please make sure you have the latest version of the software. 
There are two files that are needed as an input 
<li>(1) <strong> genotype file</strong> in a <strong>VCF</strong> format, and </li>
<li>(2) <strong> pedigree</strong> file in <strong>.txt</strong> format. A snippet of the pedigree file showing relationship between all individuals in a family or population is shown below. It is important that the pedigree file is formatted exactly as shown in the below figure: </li>
<center><img src="/image/lepmap/ped.png"></center>
<a href="https://click.linksynergy.com/fs-bin/click?id=876kEArXFCo&offerid=672566.7&subid=0&type=4" rel="nofollow"><IMG border="0"   alt="Aporro" src="https://ad.linksynergy.com/fs-bin/show?id=876kEArXFCo&bids=672566.7&subid=0&type=4&gridnum=16"></a>
<hr>


<h3>Step 1.2. Parent Call</h3>
The parental genotypes are called using the `ParentCall2` module, using the below command:

```bash
$ java -cp /path/Lep-MAP3/bin ParentCall2 data = pedigree.txt  vcfFile = File.vcf > p.call
```
<hr>
<center><img src="/image/lepmap/runlepmap.gif.png"></center>
<a href="https://click.linksynergy.com/fs-bin/click?id=876kEArXFCo&offerid=320088.10000210&type=4&subid=0" rel="nofollow"><IMG alt="Bake Me  A Wish" border="0" src="https://www.bakemeawish.com/banner/Newsletter/febaff/jumbobday_728x90.jpg"></a><IMG border="0" width="1" height="1" src="https://ad.linksynergy.com/fs-bin/show?id=876kEArXFCo&bids=320088.10000210&type=4&subid=0">
<hr>

<h3>Step 1.3. Filtering </h3>

One may use the `Filtering2` module to remove non-informative markers (Markers that are monomorphic or homozygous in both parents), and similarly, to remove distorted markers the below command line can be used to run the module:

```bash
 $ java -cp /path/Lep-MAP3/bin Filtering2 data=p.call  removeNonInformative=1 dataTolerance=0.001  > p_fil.call
```

Note: Use: `removeNonInformative` parameter to remove markers that are homozygous in both parent and `dataTolerance` to remove distorted markers at given p-value threshold.

<h3>Step 1.4. Separate Chromosomes </h3>

In this step, `SeparateChromosomes2` module is used to categorized markers into their linkage groups or chromosomes using the below command:

```bash
$ java -cp /path/Lep-MAP3/bin  SeparateChromosomes2 data=p_fil.call lodLimit=10 > map.txt
```

<h3>Step 1.5. Order Markers </h3>

In this step, markers separated into their corresponding linkage groups are ordered using `OrderMarkers2` module using the below command:

```bash
$ java -cp /path/Lep-MAP3/bin  OrderMarkers2 data=p_fil.call map=map.txt > order.txt
```
<center>
<a href="https://click.linksynergy.com/fs-bin/click?id=876kEArXFCo&offerid=659193.10002164&subid=0&type=4" rel="nofollow"><IMG border="0"   alt="Microsoft" src="https://ad.linksynergy.com/fs-bin/show?id=876kEArXFCo&bids=659193.10002164&subid=0&type=4&gridnum=13"></a>
</center>
	
One may use the parameter `sexAveraged`  to calculate sex-averaged map distances ( by default male and female genetic maps are curated separately), also `numMergeIterations` paramteres are used to adjust number of iterations (by deafault its is 6 iterations per linkage group). 

<h2> 2.0 Checking the accuracy of the marker order </h2>

If the physical positons of the markers in the genetic map curation are known, then, it is a good thing to use that information to evaluate the markers order especially markers that <strong> inflat </strong> the chromosome length, by making a correlation plot of the genetic and physical positions of the markers by chromosome. <strong>Note:</strong> It is quite common to see that the marker orders are flipped. There is nothing to panic about, one may fix it by manually sorting it.

<center><img src="/image/lepmap/corr_geneticmap.png"></center>
<a href="https://click.linksynergy.com/fs-bin/click?id=876kEArXFCo&offerid=666413.39&subid=0&type=4" rel="nofollow"><IMG border="0"   alt="UrbanStems" src="https://ad.linksynergy.com/fs-bin/show?id=876kEArXFCo&bids=666413.39&subid=0&type=4&gridnum=16"></a>
<hr>

<h2> 3.0 Converting phased output data from OrderMarkers2 to genotypes </h2>

The phased data from OrderMarkers2 step can be converted to fully informative "genotype" data by using `map2gentypes.awk` script and command below: 
```bash
	$ awk -vfullData=1 -f map2genotypes.awk order.txt > genotypes.txt
```

Snippet of the `map2gentypes.awk` output:

<center><img src="/image/lepmap/orderOutput.png">
<a href="https://click.linksynergy.com/fs-bin/click?id=iVSc/qZ7CV4&offerid=660525.52&subid=0&type=4" rel="nofollow"><IMG border="0"   alt="Real Ketones" src="https://ad.linksynergy.com/fs-bin/show?id=iVSc/qZ7CV4&bids=660525.52&subid=0&type=4&gridnum=16"></a>
</center>
<hr>

One may convert the genotypes in <strong> 1 1 => A, 2 2 => B, 1 2 or 2 1 => H format </strong> (See below figure) in MS Excel using find/Replace function, which can be then loaded in `R/Qtl` for QTL mapping.

<center><img src="/image/lepmap/rqtlFormat.png">
<a href="https://click.linksynergy.com/fs-bin/click?id=iVSc/qZ7CV4&offerid=645454.33&type=4&subid=0" rel="nofollow"><IMG alt="Lawn & Gardening Tools by Life and Home" border="0" src="https://lifeanddothomedotblog.files.wordpress.com/2019/07/lifeandhome-gardening-tools-banner.png"></a><IMG border="0" width="1" height="1" src="https://ad.linksynergy.com/fs-bin/show?id=iVSc/qZ7CV4&bids=645454.33&type=4&subid=0">
</center>
<hr>


<h2> 4.0 Validate the genetic map by conducting QTL analysis on well studied phenotype </h2>

<center><img src="/image/lepmap/qtl.png"></center>
<a href="https://click.linksynergy.com/fs-bin/click?id=876kEArXFCo&offerid=675514.47&subid=0&type=4" rel="nofollow"><IMG border="0"   alt="TW Steel UK" src="https://ad.linksynergy.com/fs-bin/show?id=876kEArXFCo&bids=675514.47&subid=0&type=4&gridnum=16"></a>
<hr>	


<h2> 5.0 Graphical presentation of linkage maps in Mapchart </h2>

Software Mapchart can be downloaded from below link.
https://www.wur.nl/en/show/Mapchart.htm

<center><img src="/image/lepmap/mapChart.png"></center>
<a href="https://click.linksynergy.com/fs-bin/click?id=876kEArXFCo&offerid=551783.18&subid=0&type=4" rel="nofollow"><IMG border="0"   alt="Qatar Airways" src="https://ad.linksynergy.com/fs-bin/show?id=876kEArXFCo&bids=551783.18&subid=0&type=4&gridnum=16"></a>

__Thank you__ for reading this tutorial. I really hope these steps will get you started in genetic map construction in Lep-MAP3. The key is to <strong> PRACTISE. </strong>. If you have any comments or suggestions, please let comment below or send me an email. 

Happy mapping !
<center><a href="https://click.linksynergy.com/fs-bin/click?id=iVSc/qZ7CV4&offerid=682323.10&subid=0&type=4" rel="nofollow"><IMG border="0"   alt="Gelato Pique - New Arrivals" src="https://ad.linksynergy.com/fs-bin/show?id=iVSc/qZ7CV4&bids=682323.10&subid=0&type=4&gridnum=13"></a>
</center>

<h3> Bibliography </h3>
Rastas, Pasi. "Lep-MAP3: robust linkage mapping even for low-coverage whole genome sequencing data." Bioinformatics 33.23 (2017): 3726-3732.
<a href="https://click.linksynergy.com/fs-bin/click?id=iVSc/qZ7CV4&offerid=595115.98&subid=0&type=4" rel="nofollow"><IMG border="0"   alt="The Trailer Parts Outlet 5% OFF coupon code SUMMER5" src="https://ad.linksynergy.com/fs-bin/show?id=iVSc/qZ7CV4&bids=595115.98&subid=0&type=4&gridnum=16"></a>
<hr>

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
