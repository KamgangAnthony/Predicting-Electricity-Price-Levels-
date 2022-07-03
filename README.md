# Predicting Electricity Price Levels: Project Overview 
Making a forecast of electricity price levels in Australia

* Created a model that can predict electricity prices for the next day in Australia, given prices for the past 7 days 
* For a company that generates electricity(through solar panels for example), they can store when prices are low and sell when they are high
* The model is 64% accurate(Pretty high given that prices are even affected by political reasons for example!):(MAPE ~36%)
* Analysed and reported how the price varies per season and which factors affect it.
* Analysed the profitability of storing electrical power in a 70MWh battery system and resell them when prices go high
* Optimized Linear, Random Forest, Gradient boosting regressor models using two ensembling methods(bagging and stacking) to reach the best model.
* The model predicts the price. Will maybe create an API using flask for this project in the future.



## Code and Resources Used 
**Python Version:** 3.9.12 <br>
**Packages:** pandas, numpy, sklearn, matplotlib, seaborn, lightbgm, scipy.stats 


## Data from an Australian company:

* "date" - from January 1, 2015, to October 6, 2020.
* "demand" - daily electricity demand in MWh.
* "price" - recommended retail price in AUD/MWh. (AUD is australian dollar :)
* "demand_pos_price" - total daily demand at a positive price in MWh.
* "price_positive" - average positive price, weighted by the corresponding intraday demand in AUD/MWh.
* "demand_neg_price" - total daily demand at a negative price in MWh.
* "price_negative" - average negative price, weighted by the corresponding intraday demand in AUD/MWh.
* "frac_neg_price" - the fraction of the day when the demand traded at a negative price.
* "min_temperature" - minimum temperature during the day in Celsius.
* "max_temperature" - maximum temperature during the day in Celsius.
* "solar_exposure" - total daily sunlight energy in MJ/m^2.
* "rainfall" - daily rainfall in mm.
* "school_day" - "Y" if that day was a school day, "N" otherwise.
* "holiday" - "Y" if the day was a state or national holiday, "N" otherwise. 

## Data Cleaning
After getting the data, I needed to clean it up so that it was usable for our model. I made the following changes and created the following variables:

*	Renamed RRP to price
*	Made columns for year, month, day and week from the date
*	Converted solar exposure from MJ/m^2 to MWh/m^2
*	Get prices for the 7 previous days and store them as new columns(price_1 -> price_7)
*	Added a new column(median_price) that has the median price of electricity for the past 7 days for each date in data.
*	Added a new column(historical_median) that has the median historical price by week for each date using only data that is before the date
*	Added a new column(historical_demand) that has the median historical demand by week for each date using only data that is before the date
*	Added 3 columns for tomorrow's price, fill batteries or not, the profit.



## EDA
I looked at the distributions of the data and the value counts for the various categorical variables. Below are a few highlights from the pivot tables.
* Demand per year
![alt text](https://github.com/KamgangAnthony/Predicting-Electricity-Price-Levels-/blob/main/photos/Screenshot%202022-07-03%20160233.png "Demand per year")

* Price per year
![alt text](https://github.com/KamgangAnthony/Predicting-Electricity-Price-Levels-/blob/main/photos/Screenshot%202022-07-03%20161956.png "Price per year")

* Price per month
![alt text](https://github.com/KamgangAnthony/Predicting-Electricity-Price-Levels-/blob/main/photos/Screenshot%202022-07-03%20160528.png "Price per month")

* Correlation between price and demand
![alt text](https://github.com/KamgangAnthony/Predicting-Electricity-Price-Levels-/blob/main/photos/Screenshot%202022-07-03%20160928.png "Correlation between price and demand")

## Model Building 

First, I transformed the categorical variables into dummy variables. I also split the data into train and tests sets with a test size of 20%.   

I tried three different models and evaluated them using Mean Absolute Error. I chose MAE because it is relatively easy to interpret and outliers aren’t particularly bad in for this type of model.   

I tried three different models:
*	**Multiple Linear Regression** – Baseline for the model
*	**Lasso Regression** – Because of the sparse data from the many categorical variables, I thought a normalized regression like lasso would be effective.
*	**Random Forest** – Again, with the sparsity associated with the data, I thought that this would be a good fit. 

## Model performance
The Random Forest model far outperformed the other approaches on the test and validation sets. 
*	**Random Forest** : MAE = 11.22
*	**Linear Regression**: MAE = 18.86
*	**Ridge Regression**: MAE = 19.67

## Productionization 
In this step, I built a flask API endpoint that was hosted on a local webserver by following along with the TDS tutorial in the reference section above. The API endpoint takes in a request with a list of values from a job listing and returns an estimated salary. 



