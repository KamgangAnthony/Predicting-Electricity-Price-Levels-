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
I looked at the manner in which price and demand vary. Below are a few highlights from the data.
* Demand per year
<img src="https://github.com/KamgangAnthony/Predicting-Electricity-Price-Levels-/blob/main/photos/Screenshot%202022-07-03%20160233.png" alt="Logo" width=600 height=300>

* Price per year
<img src="https://github.com/KamgangAnthony/Predicting-Electricity-Price-Levels-/blob/main/photos/Screenshot%202022-07-03%20161956.png" alt="Logo" width=600 height=300>

* Price per month
<img src="https://github.com/KamgangAnthony/Predicting-Electricity-Price-Levels-/blob/main/photos/Screenshot%202022-07-03%20160528.png" alt="Logo" width=600 height=300>

* Correlation between price and demand
<img src="https://github.com/KamgangAnthony/Predicting-Electricity-Price-Levels-/blob/main/photos/Screenshot%202022-07-03%20160928.png" alt="Logo" width=600 height=300>


## Model Building 

First, I transformed the categorical variables into boolean variables. I also split the data into train and tests sets and in folds for cross validation
making sure each fold during fit does not use future values.

I tried five different models and based on their results, I tried to ensemble methods. Finally the stacking ensemble gave the lowest MAPE. I chose MAPE because it is a good KPI to measure forecast accuracy.  

These are five models + two ensembles I tried:
*	**linear Regression**
*	**linear gradient boosting model** 
*	**gradient boosting model** 
*	**bagging regressor model** 
*	**random forest regressor** 
*	**ensemble voting regressor model** 
*	**ensemble stacked regressor model** – Combined Bagging and gradient boosting regressor to linear regression model.(Yields the best-smallest MAPE)

## Model performance
The ensemble stacked regressor model outperformed the other approaches on the test and validation sets.
*	**ensemble stacked regressor model** : MAPE = 36.1%
*	**ensemble voting regressor model**: MAPE = 38.2%
*	**linear gradient boosting model**: MAPE = 39.4%
*	The other models performed worst than those three.

## Productionization 
Now with information from the past 7 days a company can predict the next day's electricity price, buy today and sell tomorrow if tomorrow's price is greater for example. API coming in the future.


