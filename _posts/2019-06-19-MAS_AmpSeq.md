---
layout: post
title: "Marker Assisted Selection using AmpSeq data"
tags: [Marker Assisted Selection, AmpSeq, Amplicon Sequencing]
image: /image/ampseq/keepdiscard.png
share-img: /image/ampseq/keepdiscard.png
---

This tutorial video is my attempt to introduce the basics of the AmpSeq data analyis in __Marker Assisted Selection__ . 
For general information on technology behind AmpSeq please read one of my previous blog post on it at this link: https://avikarn.com/2019-04-21-AmpSeq/  

<center>
<iframe width="100%" height="300" src="https://www.youtube.com/embed/qt8cGyexXPI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>

<hr>

<h3>A few suggestions on while analyzing your AmpSeq data for MAS:</h3>

<ul>
  <li> Remove markers with unknown, uninformative or with a lot of missing data. </li>
  <li> Unvalidated desirable alleles are recommended to be validated prior to impleting those markers in production phase of the analysis</li>
  <li> Please keep an eye on markers with low read counts, especially that are homozygous, because it could be a heterozygous undercalling issue. </li>
<li> Minor suggestion, while color highlighting in MS Excel using Find and Replace function, please double check the highlighted cells. Because sometimes for example while trying to highlight allele "1", undesirable allele "94/94:1" gets highlighted - please watch out for those! </li>
  <li> Adding the "Keep/Discard" column is optional. However, I prefer having it because its very helpful in making selection on multiple traits/loci in a common genetic background. </li>
</ul>

<hr>

<center>
<img src="/image/ampseq/keepdiscard.png">
</center>

<hr>

__Thank you__ for watching this tutorial. If you have any questions or comments, please let me know.

Happy AmpSeq-ing !

Image credits: I would like to gratefully thank __Mike Colizzi__ for sharing images used in the tutorial video.

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

