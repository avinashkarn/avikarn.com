---
layout: post
title: "Utilizing Machine Learning algorithms (GLMnet and Random Forest models) for
  Genomic Prediction of a Quantitative trait"
tags: [Machine Learning, GLMNET, LASSO, Random Forest, Genomic Prediction, Quantitative traits]
image: /image/mlr_rf/plot11.JPG
share-img: /image/mlr_rf/plot11.JPG
---

In this tutorial, I used two popular machine learning algorithms: `Random Forest` and `GLMnet` for `Genomic Prediction` of a quantitative trait. There are several machine learning R packages available, however, in this tutorial i used `caret` package. The objective was to develop two models: `Random forest` and `glmnet` using real data sets consisting genetic markers, that were used as `predictors` to predict a quantitative trait (names of the vairables were disguised in this tutorial), and finally, comparing the performance of the two models by testing them on a validation set. 



## Step 1. libraries and set seed

```{r}
library(randomForest)
library(mlbench)
library(caret)
library(e1071)

set.seed(06252020)

```


## Step 2. Load input file and explore 

The genetic marker data was converted into numeric format. One can use `TASSEL` software to convert their data into numeric format, as well as numerically impute any sites with missing data. Please remember, its curicial to impute the data or else it will result an error while executing the model and or result biased model output. Additionally, I removed any missing phenotypic values from the data set. 

Download the dataset i used in this tutorial here at this link: 

```{r}
phenoGeno <- read.table("rhGeno_numericImpu_malateBlues_HI.txt", header = T, na.strings = "NA")

## Explore the data
head(phenoGeno)
tail(phenoGeno)

```
<center><img src="/image/mlr_rf/plot1.JPG"></center>



### Step 2.1 Distribution of the phenotype data

```{r}
hist(phenoGeno$phenotype)
```
<center><img src="/image/mlr_rf/plot2.png"></center>

The phenotype data is normally distributed, and no preprocessing was performed. However, I do suggest transforming the data if required.

## Step 3. Training and Test set

### Step 3.1 Splitting the data into train and test set

In the next step, the imported data was split into 60/40 split, since the dataset i have used in this tutorial is small and, therefore, 60/40 gives a larger and more reliable test set.

### Step 3.2 Randomizing the order of the dataset
```{r}
rows <- sample(nrow(phenoGeno))

phenoGeno <- phenoGeno[rows, ]

head(phenoGeno)
```
<center><img src="/image/mlr_rf/plot3.JPG"></center>

### Step 3.3 Find the rows to split

For this tutorial, data will be split into 60/40, which provides a larger and more reliable test/validation set.

```{r}

split <- round(nrow(phenoGeno) * 0.60)

train_set <- phenoGeno[1:split,]

test_set <- phenoGeno[(split + 1):nrow(phenoGeno),]
```

### Step 3.4 Confirm train and test set sizes
```{r}
# Check the ratios of training and test sets
nrow(train_set)/nrow(phenoGeno)

nrow(test_set)/nrow(phenoGeno)

# plot the two data sets
par(mfrow=c(1,2))
hist(train_set$phenotype)
hist(test_set$phenotype)
par(mfrow=c(1,1))
```
<center><img src="/image/mlr_rf/plot4.png"></center>

<hr>

## Random forest - rf function

The name `Random forest` is a popular name in the area of machine learning, and also is a cool name! This alogrithm is often acurrate than other algorithms, easier to tune, require less little preprocessing, and capture threshold effects and variale interactions, however, it is less interpretable and often quite slow.

### TrainControl object
Before creating and running the models, its a good idea to create a `trainControl` object to tuning parameters and further control how models are created.

```{r}
myTrainingControl <- trainControl(method = "cv", 
                              number = 10, 
                              savePredictions = TRUE, 
                              classProbs = TRUE, 
                              verboseIter = TRUE)
```

### Random forest model

Markers in the column from 2:1942 were used as predictors (`x`) and the `phenotype` column as the `y` variable. Similarly, the performance of the model was evaluated using root mean square error (`RMSE`) (One can choose other method such as `ROC`, `AUC` etc as well), and finally choosing random forest `rf` as the method in the model.

```{r}
# Create tunegrid for mtry to tune the model. 
tunegrid <- data.frame(.mtry=c(2,4,6, 10, 15, 20, 100))

randomForestFit = train(x = train_set[2:1942], 
                        y = train_set$phenotype, 
                        method = "rf",
                        metric = "RMSE",
                        tuneGrid = tunegrid
                        )

# print the model output 
randomForestFit
```
<center><img src="/image/mlr_rf/plot5.JPG"></center>

Plotting the `randomForetFit` model. 

```{r}
plot(randomForestFit, main = "Random Forest")
```

<center><img src="/image/mlr_rf/plot6.png"></center>

The best mtry with lowest RMSE for this model was 10. 

Next, we can plot the important predictors based on their calculated importance scores using `varImp` function.

```{r}
plot(varImp(randomForestFit), top = 20)
```
<center><img src="/image/mlr_rf/plot7.png"></center>

After building the supevised random forest learning model, The top 20 predictors can be ranked by their importance, and from the above plot, we tell that the `marker703` was the most important variable.

### Validation of Random Forest model

Now we can test the robustness of the model by testing it on `test_set` data set, and regressing the predicted and measured values.

```{r}
prediction_rf <- predict(randomForestFit, test_set)

plot(test_set$phenotype, prediction_rf,
     main = "Random Forest", pch=17, col=c("red"))
abline(lm(prediction_rf~test_set$phenotype))


```
<center><img src="/image/mlr_rf/plot8.png"></center>

And we can also calculate the `RMSE` between the predicted and measured values.

```{r}
error_rf <- prediction_rf - test_set$phenotype
rmse_rf <- sqrt(mean(error_rf^2))
rmse_rf
```



## Glmnet model - glmnet function

This is another popular machine learning algorithm, which has some benefits over the Random forest model. It is a extension of `glm` models with a built-in variable selection that also help in dealing with `collinearity` and small sizes. The `glmnet` has two primary froms: 1) `LASSO` regression, which penalizes number of non-zero coefficients, and 2) `Ridge` regression, which penalizes absolute magnitude of coefficients. It also attempts to find a parsimonious aka simple model and pairs well with random forest models.


First, we create the glmnet models, which is a combination of lasso and ridge regression, and we can fit the mix of the two models by selecting the values for `alpha` and `lambda`, as shown in the below code snippet. 

### glmnet model

```{r}
glmnetFit = train(x = train_set[2:1942], 
            y = train_set$phenotype, 
            method = "glmnet",
            metric = "RMSE",
            trainControl = myControl,
            tuneGrid = expand.grid(
              alpha=0:1,
              lambda= 0:10 / 10
            )
          )

print(glmnetFit)
```

<center><img src="/image/mlr_rf/plot9.png"></center>

From the above output of the model, we see that alpha = 1, indicating lasso regression was performed, where, the size of the penalty was 0.7 


### Comparing models: Ridge vs LASSO

```{r}
plot(glmnetFit, main = "Glmnet")

```
<center><img src="/image/mlr_rf/plot10.png"></center>

From the above comparison model, we can say that the lamda of 0.9 has lowest RMSE.

### Full regularization path
```{r}
plot(glmnetFit$finalModel)
```

<center><img src="/image/mlr_rf/plot11.JPG"></center>

Next, we can plot the important predictors in this model based on their calculated importance scores using `varImp` function.


```{r}
plot(varImp(glmnetFit), top = 20)

```

<center><img src="/image/mlr_rf/plot12.JPG"></center>


`marker703` was calculated as the most important variable by glmnet model, which is consistent with random forest model. However, the glmnet has very few variables listed in comparison to rf.

### Validation of glmnet model

Now we can test the robustness of the glmnet model by testing it on `test_set` data set, and regressing the predicted and measured values.

```{r}
prediction_glmnet <- predict(glmnetFit, test_set)

plot(test_set$phenotype, prediction_glmnet,
     main = "GLMnet", pch=17, col=c("red"))
abline(lm(prediction_glmnet~test_set$phenotype))

```
<center><img src="/image/mlr_rf/plot13.JPG"></center>

From the above plot, we see that the most important variables only classify the high and low phenotypic scores as categorical variables, which is not the nature of this phenotype data used in this tutorial.

And we can also calculate the `RMSE` between the predicted and measured values.

```{r}
error_glm <- prediction_glmnet - test_set$phenotype
rmse_glm <- sqrt(mean(error_glm^2))
rmse_glm
```

## Scatter plots of the predicted values from two models
```{r}
plot(test_set$phenotype, prediction_glmnet,
      pch=17, col=c("red"), ylab = "Predicted", xlab = "Measured")
points(test_set$phenotype, prediction_rf, pch=20, col=("blue"))
abline(lm(prediction_glmnet~test_set$phenotype), lwd=1, col="red")
abline(lm(prediction_rf~test_set$phenotype), lwd=1, col="blue")
legend(14,9, legend=c("GLMnet", "Random Forest"),
       col=c("red", "blue"), lty=1, cex=0.8)
```

<center><img src="/image/mlr_rf/plot14.JPG"></center>

## Comparing the Random Forest and Glmnet models
We can compare the performance of the models by studying their MAE, RMSE and R-squared values side-by-side, making it very convenient.

```{r}
# Create model_list
model_list <- list(random_forest = randomForestFit, glmnet = glmnetFit)

# Pass model_list to resamples(): resamples
resamples <- resamples(model_list)
resamples

# Summarize the results
summary(resamples)
```

<center><img src="/image/mlr_rf/plot15.JPG"></center>

From above table, we can tell that the random forest model appears to be better model incoparison to glmnet.


## plot models by MAE, R-squrared and RMSE

Similarly, we can visually inspect the models accuracies.
```{r}

bwplot(resamples, metric = "RMSE")
dotplot(resamples, metric = "RMSE")
```
<center><img src="/image/mlr_rf/plot16.JPG"></center>

## Conclusion:

In this tutorial, real marker and phenotype data were used in machine learning models: random forest and glmnet(lasso and ridge regression). Marker data were converted into numeric format and numerically imputed, prior to performing the supervised machine learning. Both models had a very close performance for this data set, but random forest did perform slightly better in comparison to glmet, and most importantly both models listed `marker703` as the most important variable, which is astoudingly consistent with my `QTL` analysis, that is the peak QTL marker was in LD with `marker703` explaining highest phenotypic variance.


