# Sales Forecasting

## 1. Problem Statement

SimpleBuy is a clothing company. Be it parent, child, man, woman, they have wide range of products catering to the need to every individual. They aim to become one stop destination for all clothing desires.
 
Their idea of offline and online channels is doing quite well. Their stock now runs out even faster than they could replenish it. Customers are no longer skeptical about their quality. Their offline stores help customer to physically check clothes before buying them, especially the expensive clothes. In addition, their delivery channels are known to achieve six sigma efficiency.
 
However, SimpleBuy can only provide this experience, if they can manage the stocks well. Hence, they need to forecast the sales ahead of time. And this is where you will help them in collecting the raw material and planning the manufacturing process. SimpleBuy has provided you with their Sales data for last 2 years and they want to you predict the sales for next 6 months.

### Let me Summarize all the steps teken in this project.
### 1. Overview of the overall dataset
   - We have daily sales data for two years, starting from 1-jan-07 to 24-dec-08.
   - Using this historical data, we need to forecast the demand expected in the next 6 months. 
    
### 2. Data Preprocessing
   - Clearly there are some very high values in the data. So the values beyond the upper whisker value are removed.  
   - There were also some missing dates. apart from sunday. So, using ffill method, i hav imputed the missing dates and removed sundays completely from series considering that store open from Monday to Saturday only.
   
### 3. Feature Extraction and Exploration
   - I It consist of decmposing the series into Trend, Seasonal and Resid
   - Apart from that i have extracted date related information like day, month, dayofyear, year etc. from Date column to have a better insights and findings are mentioned below:
       - Except for a few high sales in 2007, sales were comparatively higher in 2008
       - All weekdays have similar trend on sales
       - Average sales are higher towards the end of the year
   
### 4. Time Series Forecasting Models

The complete dataset is divided into two parts : Train and Validation dataset

- Validation dataset: It consist of last 6 months data from dataset
- Train dataset: It consist of remaining 1.5 year data from dataset

#### Time Series Models:

- **Holt' Winter Model( a.k.a. Traiple Exponential Smoothing)**
    - This method takes into account the three parameters such as : Level, Trend and Seasonality
    - I created grid_search function to find the optimal values of level, trend and seasonality and the retrained the model with these optimized hyper-parameters.
```
The minimum RMSLE observed with Hot Winter Model: 0.58
```
---
- **ARIMA Model**
    - This method consist of three componensts suc as AR, I and MA. AR menas autoregressive, I means integration (differencing) and MA means Moving average.
    - AR, I and MA are represented by p,d and q parameters in this model.
    - The values of p,d and q can be found out by various methods such as:
        - By using ACF and PACF plots
        - By usinf auto_arima  module
        - By iterations
    - I used iteration method and created grid_search function to find the optimal values of p,d and q and retrained the model with these optimized hyper-parameters.
    - It fails to caputure the seaonality in series
```
The minimum RMSLE observed with ARIMA Model: 0.57
```   
---
- **SARIMA Model**
    - This method is same as ARIMA model but in addition to that , a seasonal component is added that add four additional pararmeters such as P, D, Q and s which represent AR, I, MA and seasonality for seasonal components also.
    - AR, I and MA are represented by p,d and q parameters in this model.
    - The values of non seasonal componenets(p,d and q) and seasonal components (P,D,Q,s) can be found out by various methods such as:
        - By using ACF and PACF plots
        - By usinf auto_arima  module
        - By iterations
    - I used iteration method and created grid_search function to find the optimal values of non seasonal componenets(p,d and q) and seasonal components (P,D,Q,s) and retrained the model with these optimized hyper-parameters.
    - It also captured the seasonaltiy of series 
```
The minimum RMSLE observed with SARIMA Model: 0.57
```

### 4. Machine Learning Models
- **Linear Regression**
    - It also captured the seasonaltiy of series to some extent
```
The minimum RMSLE observed with Linear Regression Model: 0.64
```  
---    
- **Ridge Regression**
    - The alpha value is tuned and get final alpha value 0.5 w.r.t. least error
    - It also captured the seasonaltiy of series to some extent
```
The minimum RMSLE observed with Ridge Regression Model: 0.57
```    
---    
- **Random Forest**
    - I have created custom grid_search_random_forest function to find out the best hyperparameters.
    - The best hyperparameters i got after tuning are:
        - n_estimators=150
        - max_depth=6
        - min_samples_split = 50
    - It also captured the seasonaltiy of series.
```
The minimum RMSLE observed with Random Forest Regression Model: 0.55
```        
---
- **XGB Regressor**
    - I have created custom grid_search_xgb function to find out the best hyperparameters.
    - The best hyperparameters i got after tuning are:
        - n_estimators=70
        - max_depth=5
        - learning_rate = 0.3
    - It also captured the seasonaltiy of series
```
The minimum RMSLE observed with Random Forest Regression Model: 0.57
```    

# Conclusion
Out of all the models, Random Forest preformed best with respect to this particular problem so we can go with this model for next six months forecasting
