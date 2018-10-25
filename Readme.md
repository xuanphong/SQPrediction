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
# Time Series Regression
```
> SQ <- read.csv("SQ-Data.csv", header = TRUE)
> head(SQ)
        Date   SP500 DOWJONES  NASDAQ RUSSELL    SQ    SQVOL
1 2018-03-01 2677.67 24608.98 7180.56 1507.39 46.01 17570500
2 2018-03-02 2691.25 24538.06 7257.87 1533.17 46.02 11010500
3 2018-03-05 2720.94 24874.76 7330.71 1546.05 50.42 30623200
4 2018-03-06 2728.12 24884.12 7372.01 1562.20 49.60 20827200
5 2018-03-07 2726.80 24801.36 7396.65 1574.53 50.72 18318000
6 2018-03-08 2738.97 24895.21 7427.95 1571.97 52.25 16961300
> str(SQ)
'data.frame':	153 obs. of  7 variables:
 $ Date    : Factor w/ 153 levels "2018-03-01","2018-03-02",..: 1 2 3 4 5 6 7 8 9 10 ...
 $ SP500   : num  2678 2691 2721 2728 2727 ...
 $ DOWJONES: num  24609 24538 24875 24884 24801 ...
 $ NASDAQ  : num  7181 7258 7331 7372 7397 ...
 $ RUSSELL : num  1507 1533 1546 1562 1575 ...
 $ SQ      : num  46 46 50.4 49.6 50.7 ...
 $ SQVOL   : num  17570500 11010500 30623200 20827200 18318000 ...
> SQ <- mutate(SQ, MonthYear = paste(year(Date),formatC(month(Date), width = 2, flag = "0")))
> SQ <- mutate(SQ, Yearday = paste(year(Date), formatC(month(Date), width = 2, flag = "0"),formatC(day(Date), width = 2, flag = "0")))
> SQ <- mutate(SQ, Week = week(Date))
> SQ <- mutate(SQ, Year = year(Date))
> SQ$Year <- as.factor(SQ$Year)
> str(SQ)
'data.frame':	153 obs. of  11 variables:
 $ Date     : Factor w/ 153 levels "2018-03-01","2018-03-02",..: 1 2 3 4 5 6 7 8 9 10 ...
 $ SP500    : num  2678 2691 2721 2728 2727 ...
 $ DOWJONES : num  24609 24538 24875 24884 24801 ...
 $ NASDAQ   : num  7181 7258 7331 7372 7397 ...
 $ RUSSELL  : num  1507 1533 1546 1562 1575 ...
 $ SQ       : num  46 46 50.4 49.6 50.7 ...
 $ SQVOL    : num  17570500 11010500 30623200 20827200 18318000 ...
 $ MonthYear: chr  "2018 03" "2018 03" "2018 03" "2018 03" ...
 $ Yearday  : chr  "2018 03 01" "2018 03 02" "2018 03 05" "2018 03 06" ...
 $ Week     : num  9 9 10 10 10 10 10 11 11 11 ...
 $ Year     : Factor w/ 1 level "2018": 1 1 1 1 1 1 1 1 1 1 ...
> SQ$Date <- strptime(SQ$Date, "%Y-%m-%d" )
> SQ$Date <- as.POSIXct(SQ$Date)
> str(SQ)
'data.frame':	153 obs. of  11 variables:
 $ Date     : POSIXct, format: "2018-03-01" "2018-03-02" "2018-03-05" "2018-03-06" ...
 $ SP500    : num  2678 2691 2721 2728 2727 ...
 $ DOWJONES : num  24609 24538 24875 24884 24801 ...
 $ NASDAQ   : num  7181 7258 7331 7372 7397 ...
 $ RUSSELL  : num  1507 1533 1546 1562 1575 ...
 $ SQ       : num  46 46 50.4 49.6 50.7 ...
 $ SQVOL    : num  17570500 11010500 30623200 20827200 18318000 ...
 $ MonthYear: chr  "2018 03" "2018 03" "2018 03" "2018 03" ...
 $ Yearday  : chr  "2018 03 01" "2018 03 02" "2018 03 05" "2018 03 06" ...
 $ Week     : num  9 9 10 10 10 10 10 11 11 11 ...
 $ Year     : Factor w/ 1 level "2018": 1 1 1 1 1 1 1 1 1 1 ...
> head(SQ)
        Date   SP500 DOWJONES  NASDAQ RUSSELL    SQ    SQVOL MonthYear    Yearday Week Year
1 2018-03-01 2677.67 24608.98 7180.56 1507.39 46.01 17570500   2018 03 2018 03 01    9 2018
2 2018-03-02 2691.25 24538.06 7257.87 1533.17 46.02 11010500   2018 03 2018 03 02    9 2018
3 2018-03-05 2720.94 24874.76 7330.71 1546.05 50.42 30623200   2018 03 2018 03 05   10 2018
4 2018-03-06 2728.12 24884.12 7372.01 1562.20 49.60 20827200   2018 03 2018 03 06   10 2018
5 2018-03-07 2726.80 24801.36 7396.65 1574.53 50.72 18318000   2018 03 2018 03 07   10 2018
6 2018-03-08 2738.97 24895.21 7427.95 1571.97 52.25 16961300   2018 03 2018 03 08   10 2018
> SQ_Monthly <- aggregate(SQ$SQ, by = list(SQ$MonthYear), FUN = function(x) mean(x, na.rm=T))
> My_SQ_Price <- ts(SQ_Monthly$x, frequency=12, start = c(2018, 03), end = c(2022, 03))
> plot(My_SQ_Price)
> SQ_TimeSr <- data.frame(SQPrice = My_SQ_Price, as.numeric(time(My_SQ_Price)))
> names(SQ_TimeSr) <- c("SQPrice", "time")
> SQ_TS_Lm_Model <- tslm(SQPrice~season+trend,SQ_TimeSr))
Error: unexpected ')' in "SQ_TS_Lm_Model <- tslm(SQPrice~season+trend,SQ_TimeSr))"
> SQ_TS_Lm_Model <- tslm(SQPrice~season+trend,SQ_TimeSr)
> SQ_Price_Forecast <- forecast(SQ_TS_Lm_Model,h=5)
> plot(SQ_Price_Forecast)
> SQ_Price_Forecast_h1 <- forecast(SQ_TS_Lm_Model,h=1)
> plot(SQ_Price_Forecast_h1)
> SQ_Price_Forecast_h0.05 <- forecast(SQ_TS_Lm_Model,h=0.05)
> plot(SQ_Price_Forecast_h0.05)
> My_SQ_Price <- ts(SQ_Monthly$x, frequency=12, start = c(2018, 03), end = c(2020, 03))
> SQ_TimeSr <- data.frame(SQPrice = My_SQ_Price, as.numeric(time(My_SQ_Price)))
> names(SQ_TimeSr) <- c("SQPrice", "time")
> SQ_TS_Lm_Model <- tslm(SQPrice~season+trend,SQ_TimeSr)
> SQ_Price_Forecast_h5 <- forecast(SQ_TS_Lm_Model,h=5)
> plot(SQ_Price_Forecast_h5)
> SQ_Price_Forecast_h2 <- forecast(SQ_TS_Lm_Model,h=2)
> plot(SQ_Price_Forecast_h2)
> SQ_Price_Forecast_h1 <- forecast(SQ_TS_Lm_Model,h=1)
> plot(SQ_Price_Forecast_h1)
> My_SQ_Price <- ts(SQ_Monthly$x, frequency=12, start = c(2018, 03), end = c(2019, 03))
> SQ_TimeSr <- data.frame(SQPrice = My_SQ_Price, as.numeric(time(My_SQ_Price)))
> names(SQ_TimeSr) <- c("SQPrice", "time")
> SQ_TS_Lm_Model <- tslm(SQPrice~season+trend,SQ_TimeSr)
> SQ_Price_Forecast_2019_h5 <- forecast(SQ_TS_Lm_Model,h=5)
Warning messages:
1: In qt((1 - level)/2, df) : NaNs produced
2: In qt((1 - level)/2, df) : NaNs produced
> plot(SQ_Price_Forecast_2019_h5)
> My_SQ_Price <- ts(SQ_Monthly$x, frequency=12, start = c(2018, 03), end = c(2019, 10))
> SQ_TimeSr <- data.frame(SQPrice = My_SQ_Price, as.numeric(time(My_SQ_Price)))
> names(SQ_TimeSr) <- c("SQPrice", "time")
> SQ_TS_Lm_Model <- tslm(SQPrice~season+trend,SQ_TimeSr)
> SQ_Price_Forecast_2019_h5 <- forecast(SQ_TS_Lm_Model,h=5)
> plot(SQ_Price_Forecast_2019_h5)
> SQ_Price_Forecast_2019_h5 <- forecast(SQ_TS_Lm_Model,h=12)
> SQ_Price_Forecast_2019_h12 <- forecast(SQ_TS_Lm_Model,h=12)
> plot(SQ_Price_Forecast_2019_h12)

```
# Graphs 

# III: Data Sources.
There is one source to collect the newly created data. It’s on Yahoo financial. It’s free of charge.  There are two datasets will be used for this. Besides, SQ, there are other variables are added to the dataset. SP500, NASDAQ, DOWJONES, and RUSSELLS 2000 . The date range for the data will cover from 11/15/2018 to up to date.  It could be a day before weekly report due. O would like to work with the live data because it will give the more accuracy results. 

1: https://finance.yahoo.com/quote/SQ/history?p=SQ

2: https://finance.yahoo.com/quote/%5EGSPC?p=^GSPC

3. https://finance.yahoo.com/quote/%5EDJI?p=^DJI

4. https://finance.yahoo.com/quote/%5EIXIC?p=^IXIC

5. https://finance.yahoo.com/quote/%5ERUT?p=^RUT

# IV: Conclusion:
Machine Learning Regression and Time Series Regressions methods are applied to analyze and predict the SQ stock price.  Both methods generated good results. Even thought, both results are not the same but both results are close to each other. Time Series regression provided deep into detail level. The detail is the trend and future projections of SQ stock price. Machine Learning Regression provided the overview with an estimated projection.  
The results are covered from the historical data with validated at the graph level.  Below is the up to dated SQ stock price from Yahoo finance page.  Up to dated SQ price, can use to compare with Time Series Regression graph and validated. 

# V: Difficulty, challenging.
Predicting stock price is not easy. In this project, I tried to provide the overviews and prediction models to predict SQ stock price. A big challanging in predict stock price is the acuracy of the prediction. 

