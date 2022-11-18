# Problem Overview
Paragon Real Estate is a real estate agency with licensed operations in the King County Area. They help homeowners buy/sell homes. 

Paragon wants to be able to advice it's clients about how home renovations might increase the estimated value of their homes, and approximately by what amount. 

They want to help homeowners make smarter choices about investing in their propreties so that homeowners can understand whether a renovation will be helpful for their proprety valuable.


## Business Questions
1. What kind of renovations increases the value?
        - This will give homeowners an understanding of which renovations to focus on to increase the value of their homes and which renovations to avoid.

2. What is the impact to the value of the kind of renovations identified in the first question?
        - This will provide insight to a homeowner to understand what kind of renovations to prioritise and what kind of change can they expect.
        
# Data Sources
We will be using the official King County House Sales dataset

# Data Understanding
Out of the **21 features** that this dataset has, we were able to identify **7 features** which can be helpful in answering the business questions of our client. Before we review th features that we will be using, lets look at the features we will be ignoring.

Allthough these features can be important in determining th value of the house, they are being ignored because of one or both of the reasons listed below:

1. They can't be changed through renovation
2. They are linked to other more insightful features which we have chosen instead.

The features to be ignored are: 

- `date`
- `sqft_lot`
- `waterfront`
- `view`
- `sqft_above`
- `sqft_basement`
- `yr_built`
- `yr_renovated`
- `zipcode`
- `lat`
- `long`
- `sqft_living15`
- `sqft_lot15`

The features we are carrying forward and howw they will help us answer the business questions are the following:
- `price`: This is the main target that we will bee investigating and the only sourcee to find out the house price
<br>

- `bedrooms`: This will help us identify whether increasing thee bedrooms increases the house value. This will also inform a homeowner of how important the number of bedrooms are for their house price.
<br>

- `bathrooms`: Similar to number of bedrooms, this will help homeowners identify whether adding bathrooms increases their house value.
<br>

- `sqft_living`: This will provide insights to the homeowner if increasing the living space by adding another floor or building an extension is a valuable renovation or not.
<br>

- `conditions`: This provides a qualitative and quantitative assessment of categorically grouping the condition of the house. This can provide valuable insights into how changing th condition can affect the value of the house. This can help homeowners identify exactly what kind of requirements they need to meet to jump to a better condition and help plan for more impactful renovations.
<br>

- `grade`: This is the more thorough categories of the quality of construction and the house. The condition data is related to it and as shown above, it will be important to investigate the relation of this in conjunction to to other variables in determining types and impacts of renovations.
<br>

- `floor`: This is the number of levels in a house. This is important to take forward since large architectural changes such as adding more floors can improve the gradee of the house and make way for more bedrooms, bathrooms and licing square footage.

# Data Preparation
Data was cleaned and then prepared for regression analysis.

The featurees chosen based on correlation were the following:
1. floors
2. sqft_living: log transformed
3. condition: transformed through one-hot encoding
4. grade: transformed through one-hot encoding


# Modelling

Baseline model: using sqft_living_log
Train score:    0.33965784294596224
Validation score:    0.34116912874681377

a very weak model because of the low r-squared value. A value of 0.34 means that the model can only predict 34% of the variation in the dependent variable.

Therefore we have to try adding other features to the model to investigate if the preformamce improves.

We will add all of the relevant features we prepared and compare against the baseline model.


Second Model: using all features selected at the end of Data preparation
Second Model including grade#
Train score:    0.45429709503946525
Validation score:    0.45665411022858293

With a r-squared value of 0.45, this model can predict atleast 45% of the variation in the housing prices.

This is a much better result than the baseline model using all the features that we prepared. Nonetheless, the problem is that we are not optimizing our model to the most relevant features. In order to have the best resultls, we have the option of using the Recursive Feature Elimination process to improve hone in on the bese dfeatures to use.

Final Model: Using Recursive Feature Exclusion to select the best features

feature selection:

floors:False
sqft_living_log:True
condition_Fair:False
condition_Good:True
condition_Poor:False
condition_Very Good:True
grade_desc_Better:True
grade_desc_Excellent:True
grade_desc_Fair:True
grade_desc_Good:True
grade_desc_Low:False
grade_desc_Low Average:True
grade_desc_Very Good:True

final model score: 0.4639812796704821

Looks like if we ignore floors and a few other variables, we get a slight improvement in our model where we move from predicting 45% of the variation in the prices to 46%.

We will use this model as our finall because it is giving us the best r-squared results. Ideallly, we want the r-squared values to be as high as possible so that the confidence in the model predictions is high. Generally, an r-squareed value of 0.5 and higher is considered to be a strong model. With thee value of 0.46, we can say that oour model is close to being a strong model.

OLS model summary analysis:
- All the **P>|t|** values are well under 0.05 therefore wee can be confident that the featurs selelcted are statistically significant.

- The **Cond. No.** is an indication of multi-collinearity. This number should ideeally be bellow 3. We have to investigate the multi-colinearity assumption to see whether our model violates it or not.

- The **Skew** is 0 and **Kurtosis** is 3 for perfect Normal Distributions but we see a slight deviation so we will investigate the Normality Assumption too.

## Assumption Tests:

Linearity (pass): While our residuals are not prefectly linear, we can visually inspect that we are close to linearity. As the actual price tends to increase, our values are biased towards the lower end. Which means that the modele is under-predicting.

Normality (pass): We have a bell shaped-curve which means the residuals are normally distributed with a slight right skewness as we saw from our OLS results. We know from this graph, that our model is under-predicting the prices, which is something we saw in the Linearity Assumption Test also.

Multi-Colinearity (pass): 
sqft_living_log           3.123886
condition_Good            1.454836
condition_Very Good       1.138377
grade_desc_Better         1.319111
grade_desc_Excellent      1.016561
grade_desc_Fair           1.023719
grade_desc_Good           1.724535
grade_desc_Low Average    1.207339
grade_desc_Very Good      1.109697

Homoscedasticity (pass):
Homoscedasticity is the is the same variance within our error terms. Homoscedasticity can impact significance tests for coefficients due to the standard errors being biased. Additionally, the confidence intervals can be either too wide or too narrow.

While, we do not have a perfect homoscedasticity, we can see that we have the same trend that we saw before which is that it is biased towards under-prediction.





