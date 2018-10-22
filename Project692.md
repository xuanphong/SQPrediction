# Project 692
# Applied Predictive Analytics and Machine Learning in predicting stock outcome. 


# I: Overview and Data Collection.
I’m planning to apply Predictive Analytics and Machine learning technique to analyze and predict the outcome of the data.
The Data source is used SP500, NASDAQ, Dow-Jones, Russell2000, Consumer Confidence Index and Square Inc. and I select Square Inc. to predict the future stock price.  Square Inc is given the trading code name, SQ.   The source data is available in yahoo finance section. 
Yahoo finance provided historical data based on the time range.  There are 6 datasets for this project. First data is SQ dataset and second dataset is SP500. These six datasets will be combined and cleaned before starting to analyze and apply some predictive and machine learning to get an outcome.  The outcome will show the results of a prediction based on the historical and correlation with the SP500 data.  The data collection will be generated from Yahoo finance website in cvs format and import into R. The outcome will provide the information for the future invest on SQ stock.  

# II: Methodologies.
Predictive analytics and Machine learning will be applied to solve this problem.  There are graphing to visualize the outcome.  The detail Machine Learning methods, Linear Regression and Time Series are used to apply for this project.   
First, I applied linear regression to the data and I wound like to see the chance that price will increase. 
Second, I applied machine learning technique to create train and test data.  The result generated from machine learning is used to compare with historical data.  
Third, Time Series Regression is used to predict the future stock price.  There are three-time brackets to predict. Short term, it’s weekly move, medium term is 30 days and long term is about 360 days. 
There are other methods which are KNN and Neutral Network, but those two methods are optional. 

# ML Linear Regression.
```
> library(MASS)

Attaching package: ‘MASS’

The following object is masked from ‘package:dplyr’:

    select

> library(dplyr)
> library(MASS)
> set.seed(50)
> row.number <- sample(1:nrow(SQ), 0.8*nrow(SQ))
> train = SQ[row.number,]
> test = SQ[-row.number,]
> dim(train)
[1] 122  11
> dim(test)
[1] 31 11
> model1 = lm(log(SQ)~ SP500 + DOWJONES + NASDAQ + RUSSELL + SQVOL, data=train)
> summary(model1)

Call:
lm(formula = log(SQ) ~ SP500 + DOWJONES + NASDAQ + RUSSELL + 
    SQVOL, data = train)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.178757 -0.041028 -0.008581  0.047381  0.162745 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept) -2.309e+00  2.960e-01  -7.802 2.93e-12 ***
SP500        1.467e-03  7.162e-04   2.049   0.0427 *  
DOWJONES     2.175e-05  4.509e-05   0.482   0.6304    
NASDAQ       4.804e-05  1.414e-04   0.340   0.7346    
RUSSELL      8.810e-04  3.469e-04   2.540   0.0124 *  
SQVOL        3.277e-09  1.473e-09   2.225   0.0280 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.0656 on 116 degrees of freedom
Multiple R-squared:  0.9177,	Adjusted R-squared:  0.9142 
F-statistic: 258.8 on 5 and 116 DF,  p-value: < 2.2e-16

> par(mfrow=c(2,2))
> plot(model1)
> pred1 <- predict(model1, newdata = test)
> rmse <- sqrt(sum((exp(pred1) - test$SQ)^2)/length(test$SQ))
> c(RMSE = rmse, R2=summary(model1)$r.squared)
     RMSE        R2 
5.3709425 0.9177394 
> par(mfrow=c(1,1))
> plot(test$SQ, exp(pred1))
```


# III: Data Sources.
There is one source to collect the newly created data. It’s on Yahoo financial. It’s free of charge.  There are two datasets will be used for this. Besides, SQ, there are other variables are added to the dataset. SP500, NASDAQ, DOWJONES, and RUSSELLS 2000 . The date range for the data will cover from 11/15/2018 to up to date.  It could be a day before weekly report due. O would like to work with the live data because it will give the more accuracy results. 
1: https://finance.yahoo.com/quote/SQ/history?p=SQ
2: https://finance.yahoo.com/quote/%5EGSPC?p=^GSPC
3. https://finance.yahoo.com/quote/%5EDJI?p=^DJI
4. https://finance.yahoo.com/quote/%5EIXIC?p=^IXIC
5. https://finance.yahoo.com/quote/%5ERUT?p=^RUT


# IV: Difficulty, challenging.
Predicting stock price is not easy. In this project, I tried to provide the overviews and prediction models to predict SQ stock price. A big challanging in predict stock price is the acuracy of the prediction. 

