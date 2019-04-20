---
layout: post
title: "Call GBS SNPs in 7 steps using TASSEL GBSv2 pipeline"
tags: [GBS, SNP calling, TASSEL, Bioinformatics]
image: /image/GBS/GBS.png
share-img: /image/GBS/GBS.png
---

__Genotype-by-Sequencing (GBS)__ is reduced representation of a genome, which utilizes restriction enzymes (e.g. ApeKI) and NextGen sequencing to identify biallelic markers and presence/absence markers. 
In this post, my attempt is to simplify the GBS SNP calling process in 7 steps using the TASSEL GBSv2 pipeline. However, <strong>Buckler et al. </strong> provides a very well written documentation to run the SNP calling pipeline at https://www.maizegenetics.net/tassel

<center> <h2> Flowchart of the GBSv2 SNP calling pipeline </h2></center>
<center><img src="/image/GBS/gbsv2pipeline.png"></center>

<h2> Step 1. Preparing files and creating folders </h2>
To get started, create four folders: <strong> fastq  key  output  referenceGenome </strong>, using the command below:
```bash
$ mkdir fastq  key  output  referenceGenome
```
__1.1__ Place the GBS sequencing files (.fastq.gz) files in the <strong> fastq </strong> folder. Please remember the file names has to be in this fromat * flowcell_lane_fastq.txt.gz * If your fastq files does not have <strong>.fastq.txt.gz </strong> extenion, then pleae re-name them. 

__1.2__ Prepare the *Key file* with headers and information shown below figure, and place the file in the <strong> key </strong> folder.
<center><img src="/image/GBS/keyfile.png"></center>

__1.3__ Download the reference genome file and place it in the <strong> referenceGenome </strong> folder.

<h2> Step 2. GBSSeqToTagDBPlugin</h2>
In this step, *GBSSeqToTagDBPlugin* identifies tags and the taxa from the fastq files and store in the local database. Command:
```bash
$ /programs/tassel-5-standalone_20180419/run_pipeline.pl -Xms20G -Xmx50G -fork1 -GBSSeqToTagDBPlugin -e ApeKI -i fastq/ -db output/GBSV2.db -k key/keyFile_160_271.txt -kmerLength 64 -minKmerL 20 -mnQS 20 -mxKmerNum 100000000 -endPlugin -runfork1
```
In the above command, ApeKI = enzyme used in the library preparation; GBSV2.db = the name of the local database.

<h2> Step 3. TagExportToFastqPlugin</h2>
In this step, *TagExportToFastqPlugin* is used to retrieve the distinct tags stored in the <strong>GBSV2.db</strong> database, and reformmated to the fastq tags, which can be read by the *Bowtie2* aligner program. The output is a *.sam* file . Command:
```bash
$ /programs/tassel-5-standalone_20180419/run_pipeline.pl -Xms20G -Xmx50G -fork1 -TagExportToFastqPlugin -db output/GBSV2.db -o output/tagsForAlign.fa.gz -c 1 -endPlugin  -runfork1
```
#Run Alignment Program(s)
	[ak956@cbsudesktop04 Jason_GBS_SNPcalling_160_271]$ bwa index -a bwtsw referenceGenome/Noirv2.upper.fa

	[ak956@cbsudesktop04 Jason_GBS_SNPcalling_160_271]$ bwa aln -t 12 referenceGenome/Noirv2.upper.fa output/tagsForAlign.fa.gz > tagsForAlign.sai

	[ak956@cbsudesktop04 Jason_GBS_SNPcalling_160_271]$ bwa samse referenceGenome/Noirv2.upper.fa  tagsForAlign.sai output/tagsForAlign.fa.gz > tagsForAlign.sam

	[ak956@cbsudesktop04 Jason_GBS_SNPcalling_160_271]$ bowtie2-build referenceGenome/Noirv2.upper.fa PN40024v2

	[ak956@cbsudesktop04 Jason_GBS_SNPcalling_160_271]$ bowtie2  -p 15 --very-sensitive -x referenceGenome/PN40024v2/PN40024v2 -U output/tagsForAlign.fa.gz -S tagsForAlignFullvs.sam

	595364 reads; of these:
	  595364 (100.00%) were unpaired; of these:
		81305 (13.66%) aligned 0 times
		340723 (57.23%) aligned exactly 1 time
		173336 (29.11%) aligned >1 times
	86.34% overall alignment rate

#SAMToGBSdbPlugin
	[ak956@cbsudesktop04 Jason_GBS_SNPcalling_160_271]$ /programs/tassel-5-standalone_20180419/run_pipeline.pl -Xms20G -Xmx50G -fork1 -SAMToGBSdbPlugin -i tagsForAlignFullvs.sam -db output/GBSV2.db -aProp 0.0 -aLen 0  -endPlugin  -runfork1

	Total number of cut sites: 250131
	Number of cut sites with 1 tag: 130620
	Number of cut sites with 2 tags: 55554
	Number of cut sites with 3 tags: 30647
	Number of cut sites with more than 3 tags: 33310
	[pool-1-thread-1] INFO net.maizegenetics.analysis.gbs.v2.SAMToGBSdbPlugin - Finished reading SAM file and adding tags to DB.
	Total number of tags mapped: 514059 (total mappings 514059)
	Tags not mapped: 81305
	Tags dropped due to minimum mapq value: 0

#DiscoverySNPCallerPluginV2 
	[ak956@cbsudesktop04 Jason_GBS_SNPcalling_160_271]$ /programs/tassel-5-standalone_20180419/run_pipeline.pl -Xms20G -Xmx50G -fork1 -DiscoverySNPCallerPluginV2 -db output/GBSV2.db -sC "Noirv2.chr1" -eC "Noirv2.chr19" -mnLCov 0.1 -deleteOldData true  -endPlugin  -runfork1

#ProductionSNPCallerPluginV2
	[ak956@cbsudesktop04 Jason_GBS_SNPcalling_160_271]$ /programs/tassel-5-standalone_20180419/run_pipeline.pl -Xms20G -Xmx50G -fork1 -ProductionSNPCallerPluginV2 -db output/GBSV2.db -e ApeKI -i fastq/ -k key/keyFile_160_271.txt -kmerLength 64 -o 160_271_Londo_041919.vcf  -endPlugin  -runfork1


to help anyone who would like to get started to build a decent genetic map in an open software <a href="https://sourceforge.net/projects/lep-map3/"> Lep-MAP3 </a>, and finally, the evaluating the accuarcy of the map and plotting it.

The steps invloved in the genetic mapping process in Lep-MAP3 are shown in the flow chart below. 
<center><img src="/image/lepmap/lepmap_flow.png"></center>

<h3>Step 1.1. Installation and File Preparation</h3>
<strong> Important - </strong> Correctly install the Lep-MAP3 software on your computer, and please make sure you have the latest version of the software. 
There are two files that are needed as an input - (1) <strong> genotype file</strong> in a VCF format, and (2), <strong>pedigree </strong> file in .txt format. A snippet of the pedigree file showing relationship between all individuals in a family or population is shown below. It is important that the pedigree file is formatted exactly as shown in the below figure 
<center><img src="/image/lepmap/ped.png"></center>

<h3>Step 1.2. Parent Call</h3>
The parental genotypes are called using the *ParentCall2* module, using the below command:

```bash
$ java -cp /path/Lep-MAP3/bin ParentCall2 data = pedigree.txt  vcfFile = File.vcf > p.call
```

<h3>Step 1.3. Filtering </h3>
One may use the *Filtering2* module to remove non-informative markers (Markers that are monomorphic or homozygous in both parents), and similarly, to remove distorted markers the below command line can be used to run the module:

```bash
 $ java -cp /path/Lep-MAP3/bin Filtering2 data=p.call  removeNonInformative=1 dataTolerance=0.001  > p_fil.call
```
Note: Use: *removeNonInformative* parameter to remove markers that are homozygous in both parent and *dataTolerance* to remove distorted markers at given p-value threshold.

<h3>Step 1.3 Separate Chromosomes </h3>
In this step, *SeparateChromosomes2* module is used to categorized markers into their linkage groups or chromosomes using the below command:

```bash
$ java -cp /path/Lep-MAP3/bin  SeparateChromosomes2 data=p_fil.call lodLimit=10 > map.txt
```

<h3>Step 1.4 Order Markers </h3>
In this step, markers separated into their corresponding linkage groups are ordered using *OrderMarkers2* module using the below command:

```bash
$ java -cp /path/Lep-MAP3/bin  OrderMarkers2 data=p_fil.call map=map.txt > order.txt
```
One may use the parameter *sexAveraged*  to calculate sex-averaged map distances ( by default male and female genetic maps are curated separately), also *numMergeIterations* paramteres are used to adjust number of iterations (by deafault its is 6 iterations per linkage group). 

<h2> 2.0 Checking the accuracy of the marker order </h2>
If the physical positons of the markers in the genetic map curation are known, then, it is a good thing to use that information to evaluate the markers order by making a correlation plot of the genetic and physical positions of the markers. <strong>Note:</strong> It is quite common to see that the marker orders are flipped. There is nothing to panic about, one may fix it by the sorting it.

<center><img src="/image/lepmap/corr_geneticmap.png"></center>

<h2> 3.0 Converting phased output data from OrderMarkers2 to genotypes </h2>
The phased data from OrderMarkers2 step can be converted to fully informative "genotype" data by using <strong> map2gentypes.awk </strong> script and command below: 
```bash
	$ awk -vfullData=1 -f map2genotypes.awk order.txt > genotypes.txt
```
Snippet of the map2gentypes.awk output:
<center><img src="/image/lepmap/orderOutput.png"></center>

One may convert the genotypes in <strong> 1 1 => A, 2 2 => B, 1 2 or 2 1 => H format </strong> (See below figure) in MS Excel using find/Replace function, which can be then loaded in R/Qtl for QTL mapping.

<center><img src="/image/lepmap/rqtlFormat.png"></center>

<h2> 4.0 Validate the genetic map by conducting QTL analysis on well studied phenotype </h2>
<center><img src="/image/lepmap/qtl.png"></center>

<h2> 5.0 Graphical presentation of linkage maps in Mapchart </h2>
Software Mapchart can be downloaded from below link.
https://www.wur.nl/en/show/Mapchart.htm
<center><img src="/image/lepmap/mapChart.png"></center>

__Thank you__ for reading this tutorial. I really hope these steps will get you started in genetic map construction in Lep-MAP3. The key is to <strong> PRACTISE. </strong>. 
If you have any comments or suggestions, please let comment below or send me an email. 

Happy mapping !


<h3> Bibliography </h3>
P. Rastas. Lep-MAP3: Robust linkage mapping even for low-coverage whole genome sequencing data, Bioinformatics, to appear https://doi.org/10.1093/bioinformatics/btx494

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
