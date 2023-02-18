# Housing Sales Regression in King County, WA
Alexander Daffara & Andre Barle   
![house](https://raw.githubusercontent.com/alexanderdaffara/HousingSalesRegression/main/imgs/houseStock.jpg)  

Welcome to our repository! Here we will do Exploratory Data Analysis and Linear Regression models to predict housing prices from the King County Dataset. In turn we can suggest actionable insights to solve real world business problem.  

## Tackling a Business Problem  

Phil and Nancy are partners own a house in King County and are looking to renovate their home in order to maximize the value of their home. They have some spare money and have come to the "AA Consulting" Agency to seek advice on what they should renovate in their house in order to maximize the resale value. They are looking to renovate their house with the intention of flipping it to make a profit and move into a bigger house somewhere else in King County. Our analysis is aimed to help them decide which features to renovate and by how much their home value will increase. Our statistical analysis is aimed to predict home price based on certain renovations. We will test which renovations have the biggest impact on price, and create a predictive model which can help them make an informed data-driven decision. 

## Data Understanding  

This data was pulled from the King County government website and outlines home prices along with other features of the home in a csv file. We will be importing the file into two dateframes for use in the analysis. The data is generally clean, but contains zip codes from locations outside of King County, so we will be removing those as they are not relevant to the data. The address, lat, and long fields were gathered using a third party geocoding API, which explains why there was some discrepency.   

###Method  

Tools: Sci-kit Learn's LinearRegression model, preprocessing, and statsmodels' model significance statistics    
We will be applying the fundamental machine learning algorithm that is Linear Regression to estimate the target price variable from other relevant features extracted from the dataset. The Exploratory data analysis process is very important to ensure all of the assumptions of linear regression are in place for a significant model. Since this model optimizes a linear combination of the features it is given, we can inspect the learned coeficients for each feature to give Phil and Nancy reccomendations on the renovations they will be making for their house in King County. That is, given a reasonably accurate model.

## Base model  
For initial comparison, our base model is a Simple regression using an unclean version of the highest price-correlated feature in the dataset: 'sqft_living':  
![base_mode](https://raw.githubusercontent.com/alexanderdaffara/HousingSalesRegression/main/imgs/Base%20Model.png)  

>Train R2: 0.38912017997934834  
>          Test R2: 0.3518565444743025  
>             RMSE: 695395.0428436105  
>              MAE: 403638.722185664  
>Condition Number: 6883.092953416579

> Moving Forward from here we can compare our subsequent models by to the baseline model. The Metrics R2 (R squared) measures the variance explained and RMSE (root mean squared error) is a nice metric to see, in USD$ units how far our predictions are on average,(RMSE accounts for volatility at high values better than MAE).   

![heatmap](https://raw.githubusercontent.com/alexanderdaffara/HousingSalesRegression/main/imgs/heatmap_1.png)  
From the heatmap, we can see there is a high correlation between the square footage measurements.  Which will most likely violate the assumption of independance between the features for linear regression  

## Ensuring normality assumption  
Our Target and Feature variables must be as nearly normaly distributed as possible, since the linear regression is parametric and highly sensitive to outliers  
For out target (click image to inspect further):    
![price_outliers](https://raw.githubusercontent.com/alexanderdaffara/HousingSalesRegression/main/imgs/price_outliers.png)    

And for our Features:  
![feature_outliers](https://raw.githubusercontent.com/alexanderdaffara/HousingSalesRegression/main/imgs/feature_outliers_all.png)  

## Model iteration 1:  
Using all features in the dataset:  
>         Train R2: 0.6603395424433508  
>Train Adjusted R2: 0.6603395424433508  
>          Test R2: 0.6473240905020876  
> Test Adjusted R2: 0.6473240905020876  
>              MSE: 263128467132.30054  
>             RMSE: 512960.49275972566  
>              MAE: 262655.7777069954  
>Condition Number: 2295809634200880.0  

## Model iteration 3:  
By this point we have:  
 - cleaned The categorical variables to be represented as numbers for machine interpretability (YES/NO - > 1/0 and so on)  
 - normalized the numerical and categorical features by means of transforming to a Gaussian Curve  
 - linearly combined features that are highly correlated 'sqft_living', 'sqft_above', 'sqft_garage','sqft_patio', and 'sqft_basement' into one 'sqft_home'  
 - attempting to use the log transformation of our target variable and have our model predict these scaled down values  
 
>         Train R2: 0.7101373438015719  
>Train Adjusted R2: 0.7101373438015719  
>          Test R2: 0.6939157650664806  
> Test Adjusted R2: 0.6939157650664806  
>             RMSE: 661244.3444810997  
>              MAE: 220193.2860215568  
>Condition Number: 2295809634200880.0  

There was significant improment getting up to R2 = .71, but as we see with the following models, the un-transformed and outlier stripped price target were more accturate at expaiing the high end of the model's variaition.  

## Final Model Iteration  
![final_model](https://raw.githubusercontent.com/alexanderdaffara/HousingSalesRegression/main/imgs/final_resid.png)  
>Train R2: 0.7605936383412051  
>Train Adjusted R2: 0.758698426332945  
>          Test R2: 0.7815262346516275  
> Test Adjusted R2: 0.7815262346516275  
>MSE: 70101052890.4598  
>RMSE: 264766.0342461997  
>MAE: 174192.4507748189  

Our final model reached a maximum of .781 R2, with RMSE of around 264766!    

This model applied the modelling from iteration 3 as well as:  
using interactions only on a polynomial transformation | features (x0,x1) -> features (x0, x1, x0*x1), helping reduce multicolinearity, and Standard Scaling All the features, incliding the zip codes.  

looking at the Variance Inflation Factor for our final model, all features are < 5, which is a solid benchmark to determine the assumption of independance within the features are covered  
![vif](https://raw.githubusercontent.com/alexanderdaffara/HousingSalesRegression/main/imgs/VIF.png)  

# Final Recommendations:  
Calculating from the coeficients we determined these factors have the highest impact in Home sale price.  
![price-increments](https://raw.githubusercontent.com/alexanderdaffara/HousingSalesRegression/main/imgs/PriceIncrements.png)    

Considering Phil and Nancy's budget, They may be connected with one of our certified contractors to improve the contruction material grade of their home to boost their price by ~ $80,000 dollars. Keeping in mind these show increase in price per unit increase in each feature, if it is more expensive to renovate your home by increasing the grade, it may be wiser to, say, add a bathroom for a ~$65,000 price boost.    

One potential way to improve the accuracy of our model is by incorporating additional data points, including variables such as crime rates, educational attainment, and demographic characteristics. While our current model has shown some success in predicting outcomes, we acknowledge that there may still be a considerable margin of error.    

It's important to note that the data we collected was collected during the unprecedented and challenging circumstances of the Covid-19 pandemic. Given the unique conditions that were present during this time, it's possible that our results could be skewed to some degree. As such, it's important to exercise caution and recognize that our model may not be entirely representative of the current market conditions. It's possible that additional data collection and analysis will be necessary to gain a more accurate and comprehensive understanding of the current landscape.  

# Thank you!  
*Happy renovating*  

![house](https://media.istockphoto.com/id/1253610171/photo/construction-workers-working-on-wooden-roof-of-house.jpg?s=612x612&w=0&k=20&c=-AeXNrI7gRmRC3aBKSxiQoqB0eJO16nmmI1wh2iL55E=)
