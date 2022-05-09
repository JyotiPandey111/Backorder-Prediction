# Backorder-Prediction
## Description:

Backorders are unavoidable, but by anticipating which things will be backordered, planning can be streamlined at several levels, preventing unexpected strain on production, logistics, and transportation. ERP systems generate a lot of data (mainly structured) and also contain a lot of historical data; if this data can be properly utilized, a predictive model to forecast backorders and plan accordingly can be constructed. Based on past data from inventories, supply chain, and sales, classify the products as going into backorder (Yes or No).

## Problem Statement:
Classify the products whether they would go into Backorder based on the historical data from inventory, supply chain and sales

## Source/Useful Links:

#### Some articles and reference blogs about the problem statement
1. https://medium.com/analytics-vidhya/backorder-prediction-d4f1c5362f18#:~:text=A%20Backorder%20is%20an%20order,replenishment%20of%20inventory%20is%20underway
2. https://www.researchgate.net/publication/319553365_Predicting_Material_Backorders_in_Inventory_Management_using_Machine_Learning
3. https://journalofbigdata.springeropen.com/articles/10.1186/s40537-020-00345-2
4. https://www.researchgate.net/publication/327752791_Demand_Forecasting_Using_Artificial_Neural_NetworksA_Case_Study_of_American_Retail_Corporation
5. https://machinelearningmastery.com/bagging-and-random-forest-for-imbalanced-classification/
6. https://heartbeat.fritz.ai/resampling-to-properly-handle-imbalanced-datasets-in-machine-learning-64d82c16ceaa


#### Data_Source:
- https://github.com/rodrigosantis1/backorder_prediction/blob/master/dataset.rar
- We have two data files: one conatins containing training data and other containing test data



## Exploratory Data Analysis and Visualization

* 1. **Dataset Analysis:** 
     * 1.1 Missing Values
     * 1.2 Features in the dataset
     * 1.3 Class Imbalance Check
     
* 2. **Numerical Feature Statistics and Observations:**
     * 2.1 Different Statistics of the Numerical Features using Pandas Describe Function 
     * 2.2 Histogram of Continous Features in the Dataset
     * 2.3 KDE plot for clear visualization of the distribution
     * 2.3 Boxplot between Continous Features and target value "went_on_backorder"
     * 2.4 Pair Plots between Selected Features for Entire Data and Data between 5th and 90th Quantiles
     
* 3. **Categorical Feature Statistics and Observations:**
     * Bar plots between categorical features and target value "went_on_backorder"
     
* 4. **Observations from Exploratory Data Analysis and Visualization** which will be taken care in the next step i.e. Feature Engineering:




## Observations Obtained about Features in the Dataset:
* In the Train dataset we are provided with 23 columns(Features) of data. Dataset has 15 columns of data type float and 8 coumns are       string ( including target variable)
     * Numerical Feature: 15 columns 
     * All the features are continuous numerical variables.
             1. national_inv : The present inventory level of the product
             2. lead_time : Transit time of the product
             3. in_transit_qty : The amount of product in transit
             4. forecast_3_month: Forecast of the sales of the product for coming 3 months 
             5. forecast_6_month: Forecast of the sales of the product for coming 6 months 
             6. forecast_9_month : Forecast of the sales of the product for coming 9 months 
             7. sales_1_month: Actual sales of the product in last 1 month
             8. sales_3_month: Actual sales of the product in last 3 months 
             9. sales_6_month: Actual sales of the product in last 6 months 
            10. sales_9_month : Actual sales of the product in last 9 months 
            11. min_bank : Minimum amount of stock recommended
            12. pieces_past_due: Amount of parts of the product overdue if any
            13. perf_6_month_avg: Product performance over past 6 months 
            14. perf_12_month_avg: Product performance over past 12 months 
            15. Local_bo_qty : Amount of stock overdue
             
     * Categorical Features: 8 columns
            1. sku(Stock Keeping unit) : The product id — Unique for each row so can be ignored
            2. Potential_issue : Any problem identified in the product/part.Different Flags (Yes/No/nan) set for the product
            3. Deck_risk: Different Flags (Yes or No or nan) set for the product
            4. oe_constraint: Different Flags (Yes or No or nan) set for the product
            5. ppap_risk: Different Flags (Yes or No or nan) set for the product
            6. stop_auto_buy: Different Flags (Yes or No or nan) set for the product
            7. rev_stop : Different Flags (Yes or No or nan) set for the product
            8. went_on_backorder: Label/Target Variable. The target variable to predict consists of two values.
               “Yes” - If the Product predicted to go to Backorder
               “No” - If the Product predicted to be not going to Backorder
               "nan" - missing values
               
So it is a Binary Classification problem.
       
The last row of the dataset consists of NaN values for all features, which will be dropped in the feature engineering section.


## Observations from Exploratory Data Analysis and Visualization:

* Challenges observed in EDA which should be taken care in Feature Engineering Part:
  * Dataset is highly imbalanced should be handled in the feature engineerig section. The problem we are solving is binary classification with very high data imbalance       with positive class being the minority.
  * Data consists of both Categorical features and numerical features. The categorical features consists of different flags with Yes or     No values. 
  * Missing Values: Missing values are present in Lead time column and -99.0 an unusual value in performance columns in which the remaining values are b/w 0 and 1. Replace -99.0 in performance columns with Nan for imputation in feature engineering section.
  * Outliers: Most of the feature are having outliers. If we consider the datapoints between 0(0.001) th quantile and 90th quantiles values most of the outliers are removed and can make some inferences.
  * Duplicate: no duplicate datapoints. As part of preprocesing and feature engineering data , dropped the first columns (Sku) which contains product ids unique for each row and also dropped the last row whch contains nan values.
  * Distribution: Almost all the numerical columns had extreme skewedness (on positive side) indicating them as outliers or also can be     useful data as sale , inventory , forecast of some products might be very high which is also proved to be true when we plotted qq       plot of normal distribution vs log(columns), Applied the log transform to the data obtained in step 10 followed by standardization to prepare another dataset .
  * **Right  skewed columns** = ['national_inv' , 'in_transit_qty' , 'forecast_3_month','forecast_6_month','forecast_9_month' ,             'sales_1_month','sales_3_month','sales_6_month','sales_9_month' , 'min_bank','pieces_past_due','local_bo_qty']
  * **Left skewed colums** = ['perf_6_month_avg', 'perf_12_month_avg'}
