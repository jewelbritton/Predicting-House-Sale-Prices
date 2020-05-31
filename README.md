## Predicting House Sale Prices with Regression and Classification Models

This project was completed with a "full stack" real estate company in mind. This imaginary company would own the entire process of buying and selling homes. To ensure they make as much money as possible, it would be important for them to know what features of a home create the greatest sales prices, and using data analysis they can better understand the value of different improvements. 

This project uses the [Ames housing data recently made available on kaggle](https://www.kaggle.com/c/house-prices-advanced-regression-techniques).

---

## Table of Contents
* [Problem Statement & Goals](#problem-statement--goals) 
* [Predicting Sale Price Based on Fixed Features](#predicting-sale-price-based-on-fixed-features)
* [Understanding the Value of Changeable Features](#understanding-the-value-of-changeable-features)
* [Predicting Abnormal Sales](#predicting-abnormal-sales)
* [Conclusion](#conclusion)

---

## Problem Statement & Goals

The real estate company is eager to apply data analysis to housing sales prices, so that they can better predict what a home will sell for. By looking at the variation in sale price based on fixed and changeable features, the company can predict a sale price based on the current condition and forecast how much different renovations will alter the sale price.

The goals of this analysis are:
* **Use fixed features of the home to predict sale price** <br>
  - looking only at aspects that cannot be easily modified (number of bedrooms, square feet, etc.) to understand which elements impact price the most <br>
  - develop a model that can predict price based on fixed features <br>
  - forecast how much a house's value will increase with various renovations <br>
  
* **Analyze changeable features of the home** <br>
  - by looking only at the features that can easily be changed (overall quality, condition, finishes, etc.), see if better predictions on sale price can be made
  
* **Predict abnormal sales** <br>
  - create a model to predict if a home will have an abnormal sale. If this can be predicted, the real estate company will be able to foresee homes that may be sold for a cheaper price through short sales or foreclosures

## Predicting Sale Price Based on Fixed Features

1. Develop an algorithm to reliably estimate the value of residential houses based on *fixed* characteristics.
2. Identify characteristics of houses that the company can cost-effectively change/renovate with their construction team.
3. Evaluate the mean dollar value of different renovations.

This way, the company can use this information to buy houses that are likely to sell for more than the cost of the purchase plus renovations.

**Conclusion:**
The best model for predicting house sale prices based on fixed features was Elastic Net CV, after taking the log of the sales prices (y values). Based on this model, the strongest predictors of price are ground floor living space, size of the garage, year a remodel or addition was completed, number of fireplaces & neighborhood. The model performs a bit better and more accurate than the models done without taking the log of sales price, but looking at the coefficients from the previous models may also be informative for the real estate company.

Before taking the log of the sales price, the best performing model was LassoCV. The coefficients can tell the company how much more or less value a house will have based on the fixed characteristics of the home.

* Each increase by 1 sqft in Ground Floor Living Area will equate to an increase of $27,704 in sale price
* A basement with the highest quality (Excellent) will equate to a $12,973 greater sale price
* A home in North Ridge Heights or North Ridge will increase sale prices by about $10,000
* Each additional car that can fit in the garage will increase the sale price by $8,800

*insert feature importance pic*

## Understanding the Value of Changeable Features

To gain better predictions on the house's sale price, I developed a model to predict the residuals from the previous regression model based on changeable features of the home. This model would help the real estate company evaluate the cost or benefits of renovating a home to improve its quality, condition, or etc.

The residuals from the first model (training and testing) represent the variance in price unexplained by the fixed characteristics. Of that variance in price remaining, how much of it can be explained by the easy-to-change aspects of the property?

**Conclusion**
When testing different models to predict the residuals of the previous model based on easy to change aspects of the home, lasso performed the best. This model was done by using the changeable features of the homes from pre-2010 as training data and 2010 sales data as test data. This data was predicting residuals from the last model (log of y - y test). However, all models that were tested had extremely low scores. The most influential coefficients were Overall Condition and Overall Quality, but all of the coefficients were quite low, suggesting that they would not have a large impact on the sale price of the home. Even the greatest coefficient, .03, would suggest that that feature would increase the sales price by just 3%.

*coefficients

I would not advise for the real estate company to use this model to predict housing prices due to the low scores and resulting coefficients. It would be best for the real estate company to judge sale price based on fixed features of the home, as shown in the model for part 1. The model in part 1 produced much more accurate predictions, and since the coefficients were much larger, those features had much stronger impact on the value of the home.

## Predicting Abnormal Sales

The `SaleCondition` feature indicates the circumstances of the house sale. From the data file, we can see that the possibilities are:

       Normal	Normal Sale
       Abnorml	Abnormal Sale -  trade, foreclosure, short sale
       AdjLand	Adjoining Land Purchase
       Alloca	Allocation - two linked properties with separate deeds, typically condo with a garage unit	
       Family	Sale between family members
       Partial	Home was not completed when last assessed (associated with New Homes)
       
If it is possible to predict which characteristics produce an abnormal sale, the real estate agency could know ahead of time which houses may have an usual sale (short sale, foreclosure, etc...) and may allow them to buy the house for a lower price. 

1. Determine which features predict the `Abnorml` category in the `SaleCondition` feature by using a classification analysis.
- Justify results.

**Conclusion:**
Through initial EDA and feature engineering, I found that there was a strong class imbalance in the data. Over 90% of the data was regular house sales, and about 10% were abnormal sales - the variable we were interested in exploring. To combat this imbalance, I used Random Under Sampler to under sample the majority class which resulted in a 50/50 split between normal and abnormal sales. 

With the new data sample, a Logistic Regression model that was tuned with GridSearch CV resulted in a mean CV score of .638. While this model is not perfect at predicting abnormal sales, it is performing better than the baseline from the resampled data, which was 50/50. By resampling we also made more accurate predictions than the original data would have allowed, since the model would have almost always predicted a normal sale since that was where the majority class was. By undersampling the data, the model is now able to guess both categories more effectively. 

It is more likely for a home to have an abnormal sale if it is not a Warranty Deed - Conventional Sale type, if it is in Northridge neighborhood, if it has not had a remodel done recently, and if it is not a new construction home being sold. This means that an abnormal sale is most likely to be influenced by the type of sale of the home and the location it is in. Since it would not be possible to know what type of sale a house would have before predicting if the sale is abnormal or not, the most accurate predictors would be the home's location, when it was remodeled, and when when the sale is taking place. 

This model is perhaps not the most reliable, and further research should be done by the real estate company before spending lots of money on a property. However this does provide a good framework on what the company can look out for.


## Conclusion

For the real estate company client, I would suggest that fixed characteristics of the home are the best predictors for sale price. With the model created for the first part of this project, it is noticeable that quality of the basement and size of the garage both have a large impact on sale price, and may be easier to add to a home to increase profit. Additionally, size of the home and location have large impacts on price. Changeable characteristics of the home are not as influential for price, but overall quality and condition of the home can benefit the seller. 

Although it can be quite difficult to predict an abnormal sale, the company can keep a close eye on properties in specific neighborhoods since location has a large impact on type of sale. Additionally, the time of year can be a good predictor, as well as when a remodel has taken place. 

Overall, the company can expect to make reasonably accurate predictions on sale prices based on fixed characteristics, and by adding some features to the home before selling, they can expect to make a large profit. 
