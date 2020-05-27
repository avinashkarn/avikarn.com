---
layout: post
title: "Correlation analysis using R Shiny Application"
tags: [R, Shiny, Shinyapps, Data products, Correlation]
image: /image/shiny/plot_corr.png
share-img: /image/shiny/plot_corr.png
---
This is a simple R `Shiny` app, where a user can upload their own data in .csv or .txt formats, and generate a correlation matrix plots that also contains density plots and coefficient of correlation values. 

## Application Overview

- Upload your own numerical data
- Define if the data have or do not have headers
- Define what separators (comma, semicolon and tab) are used in the data
- Define if the data has quotes or no quotes
- Custom pick columns to display on the correlation matrix
- User can download "Iris.csv" file to genetration the correlation plot

<iframe src="https://avinashkarn.shinyapps.io/correlation_shinyR_1/" 
        style="border: 2px solid red; width: 100%; height: 500px;">
It looks like your browser doesn't support iframes.
</iframe>
