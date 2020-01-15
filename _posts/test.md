---
title: "Genomic Selection and prediction of acylated anthocyanins in V rupestris x Horizon"
author: "Avi Karn"
date: "Jan 14, 2020"
geometry: margin=2cm
output:
  pdf_document:
    toc_depth: 3
    number_sections: true
    toc: true
  html_document:
    highlight: tango
    number_sections: true
    theme: united
    toc: true
---

# Installing R Packages 
```{r message=FALSE, warning=FALSE}
library(snow)
library(doSNOW)
library(parallel)
detectCores()
cl <- makeCluster(6, type = "SOCK")
registerDoSNOW(cl)

library(rrBLUP)
library(dplyr)

```

# Import marker and phenotype data
```{r}
markers_RH <- read.table(file="markers_GS_RH.txt", header=T)
pheno_RH <- read.table(file = "pheno_AvgAcy_GS_RH.txt", header = T)

#head(markers_RH)
#head(pheno_RH)

dim(markers_RH)
dim(pheno_RH)
```


# Assigning Training and Testing sets
```{r}
train_set = as.matrix(sample(1:107, 80))
test_set = setdiff(1:107, train_set)

# Training data set
pheno_train_set = as.matrix(pheno_RH[train_set,])
markers_train_set = as.matrix(markers_RH[train_set,])

# Testing data set
pheno_test_set = as.matrix(pheno_RH[test_set,])
marker_test_set = as.matrix(markers_RH[test_set,])

```

# Training Model
```{r}

trained_model = mixed.solve(y=pheno_train_set, Z= markers_train_set, K = NULL, SE =F, return.Hinv = F)

```

# Marker Effects

```{r}
marker_effects = as.matrix(trained_model$u)
BLUE = as.vector(trained_model$beta)
```

# Prediction model
```{r}
predicted_train = as.matrix(markers_train_set) %*% marker_effects
predicted_test = as.matrix(marker_test_set) %*% marker_effects

predicted_train_result = as.vector((predicted_train[,1])+BLUE)
predicted_test_result = as.vector((predicted_test[,1])+BLUE)

```

## Summary of Predicted
```{r}
summary(as.vector(predicted_train_result))
summary(predicted_test_result)
```

## Correaltion between the observed and predicted data
```{r}
cor(as.vector(pheno_test_set), predicted_test_result, use = "complete")
cor(as.vector(pheno_train_set), predicted_train_result, use = "complete")

plot(pheno_test_set, predicted_test_result)
plot(pheno_train_set, predicted_train_result)

write.table(pheno_test_set, file = "test_data.txt")
write.table(predicted_test_result, file = "test_predicted.txt")

library(ggplot2)
library(forcats)
library(ggstatsplot)

test_plot = read.table(file = "forPlot_test_predicted-measured.txt", header = T)
head(test_plot)

ggstatsplot::ggscatterstats(
  data = test_plot,
  x = Measured,
  y = Predicted,
  xlab = "Predicted values",
  ylab = "Measured values",
  title = "Genomic prediction of acylated anthocyanins",
  messages = FALSE
)
 
```

