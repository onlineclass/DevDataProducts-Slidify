---
title       : Guess-the-Torus
subtitle    : Nonlinear binary classification performance
author      : Onlineclass
job         : Data Science specialization student
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]     # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Application overview
- '<b><span style = "color:red">Guess-the-Torus</span></b>' is a web application 
for binary classification performance comparison of several machine learning 
algorithms    
- The published web application was developed for a single algorithm but 
implements the functionality illustrated in this presentation    
- The web application was developed in R using the '<b>shiny</b>' package and is 
published at http://onlineclass.shinyapps.io/GuessTheTorus    
- Following is the sequence of tasks performed by the application:
    - Generate two data sets (one for model training and one for test)
    - Plot a sample taken from the training data set
    - Train several classification models on the training data set and plot the 
    ROC curves of the model performance estimated on the test data set
    - Estimate the 'outcome' for the user-specified predictor values using each 
    of the trained models and compare the predictions with the computed value of 
    the 'outcome'


--- .class #id 

## Data sets

The application generates two data sets, each having 4 columns and a 
user-specified number rows. The first three columns (labeled 'x', 'y' and 'z') 
are the Cartesian coordinates of a point in the 3D space and the last column 
(a factor labeled 'outcome') has a value of 'inside' or 'outside', depending on 
whether the point is inside or outside of the torus with equation 
<span style = "color:blue"> $\( R - \sqrt{x ^ 2 + y ^ 2} \) ^ 2 + z ^ 2 = r ^ 2$ 
</span> and parameters <span style = "color:blue"> $r = 2$,  $R = 8$ </span>
![plot of chunk unnamed-chunk-1](assets/fig/unnamed-chunk-1.png) 

--- .class #id 

## Model training

- In order to preserve the responsiveness of the web interface, the only model 
trained in the simplified web application is <b>Weighted k-Nearest Neighbors</b> 
using the <span style="font-size: 16px; color: green;"><b>kknn</b></span> function in the <b>kknn</b> package
- In this presentation, the <span style="font-size: 16px; color: green;"><b>train</b></span> function from the <b>caret</b> package is 
used to train the folowing models:
    - <b>Linear Discriminant Analysis</b> (<span style="font-size: 16px; color: green;"><b>method = "lda"</b></span>) - expected to perform 
    poorly on the nonlinear and non-convex volume, LDA is used as the reference 
    model
    - <b>Decision Trees</b> (<span style="font-size: 16px; color: green;"><b>method = "rpart"</b></span>) - uses the classification trees 
    routines from the <b>rpart</b> package
    - <b>Random Forests</b> (<span style="font-size: 16px; color: green;"><b>method = "rf"</b></span>) - uses the <b>randomForest</b> package
    - <b>Stochastic Gradient Boosting</b> (<span style="font-size: 16px; color: green;"><b>method = "gbm"</b></span>) - uses the <b>gbm</b> 
    package
- All <span style="font-size: 16px; color: green;"><b>train</b></span> function calls use the default parameters - models not optimized
- The performance of the models is estimated on the test data set and the ROC 
curves plotted using the <b>ROCR</b> package



- The training and performance estimation of each model is performed with the 
following sequence of calls:
<ul>
<li style="font-size: 16px; text-align: left; color: green;"><b>xxxFit <- train(outcome ~ ., data = train.data, method = "xxx")</b></li>
<li style="font-size: 16px; text-align: left; color: green;"><b>xxxPred <- prediction(predict(xxxFit, test.data, type = "prob")<span class="Unicode">$</span>inside, test.data<span class="Unicode">$</span>outcome)</b></li>
<li style="font-size: 16px; text-align: left; color: green;"><b>xxxPerf <- performance(xxxPred, "tpr", "fpr")</b></li>
</ul>


--- .class #id 

## Model performance and Prediction

- Models' performance is illustrated in the plot containing the ROC curves (one 
for each model)    
![plot of chunk unnamed-chunk-4](assets/fig/unnamed-chunk-4.png) 
- The simplified web application (using a k-Nearest Neighbors model) allows the 
user to specify a point in the 3D space via the 3 Cartesian coordinates (x, y 
and z) and then displays the position of the point ("inside torus" or "outside 
torus") as predicted by the model and the true position (computed using the 
equation of the torus).
