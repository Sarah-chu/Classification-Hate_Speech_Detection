# Regression - Medical Insurance Cost Prediction

## Objective : 
Medical expense is a crucial topic for every US citizen as medical cost in the US is relatively high in the world. Medical needs are unaffordable to most of the population without medical insurance. Therefore medical insurance industry is a critical product for insurance companies. The aim of this study is to analyze and predict the medical insurance premium (charges) for individuals.  

This project is written in ***R***.  
For quick summary, please refer to our [powerpoint](https://drive.google.com/file/d/1NjQW8-6npV46wZWZ2HJn95Wjg5RiJ6wQ/view?usp=sharing).

## Executive Summary :
- Age, BMI , smoker, no.of children and regions are related to the medical insurance charges.
- Age, BMI and no. of children are **positively related** to the charges. 
- Smokers are charged higher than non-smokers.
- Northeast Region charges the highest, the charges would be lower in other three regions.
- The insurance company can charge their clients based on age, bmi, smoking habit, no. of children and regions.  

## Dataset :
This dataset is provided in the book "Machine Learning with R" which can be found on [Github](https://github.com/stedy/Machine-Learning-with-R-datasets) or [Kaggle](https://www.kaggle.com/mirichoi0218/insurance). The data contains medical information and costs billed by health insurance companies. It contains 1338 rows of data and the following columns:

- age: age of primary beneficiary
- sex: insurance contractor gender, female, male
- bmi: Body mass index, providing an understanding of body, weights that are relatively high or low relative to height, objective index of body weight (kg / m ^ 2) using the ratio of height to weight, ideally 18.5 to 24.9
- children: Number of children covered by health insurance / Number of dependents
- smoker: Smoking
- region: the beneficiary's residential area in the US, northeast, southeast, southwest, northwest.
- charges: Individual medical costs billed by health insurance

## Highlights from Exploratory Data Analysis :

### Correlation between numerical variables
The darker the color, the stronger the correlation. The pie charts shows the proportion of the correlation between variables.
Relatively speaking, charges have a stronger correlation to age. The independent variables have weak correlation with each other.  
<img src="/images/corr_plot.png" width="300">

### Age and Charges
<img src="/images/chargesNage.png" width="300">  <img src="/images/Charge_age_smoker.png" width="300">  
Charges is directly proportional with age. The average charges rise when age increases.
If we take smoker factor into account, which is displayed in 2 different colours
Most smokers are charged more than non-smokers.

### Smoker and Charges
<img src="/images/Charge_smoker.png" width="300">    
The majority of the samples is non-smokers. Generally speaking, smokers pay more than non-smokers. Most of the non-smokers pay less than $18000, less than the lowest fee smokers pay. The charges for non-smokers vary from $1000 to $40000 while the charges for smokers range from $16000 to $64000.

### BMI, Smoker and Charges
<img src="/images/Charge_BMI_smoker.png" width="300">   
The distribution of BMI and charges looks like a megaphone , i.e. the charges becomes more diversified when BMI increases.
Non-smokers(blue colour) have a more concentrated distribution below $20000. Charges for smokers(pink colour) increase with their BMI.

### Sex and Charges
<img src="/images/charge_sex.png" width="300">  
The distribution of both groups are similar :  
The majority of both groups are charged less than $20000. The rest disperse between $20000 and $50000. There is a minority of outliers (around 5) beyond $50000. 

### Region and Charge
<img src="/images/charge_region.png" width="300">   
The distribution of charges against the four regions is similar. The majority of the people in each group are charged less than $20000. Some charges are around $20000 and $50000. There are outliers (1 or 2 per group) of more than $50000.

### Number of Children and Charge
<img src="/images/charge_children.png" width="300">   
The majority of the sample has 3 children or less. Those who have **5 children** are all charged **less than $20000**. Some people who have less children are charged up to $50000, with few outliers beyond this amount.


## Data Cleaning : 
1. Convert the categorical variables (Sex, Smoker, Region) into factors
2. Remove outliers in the BMI variable identified in the boxplot
3. Remove influential outliers using Cook’s distance
   - If the residual of a particular data is > 4/n (n = 1329) , i.e. >0.0030098, we remove that data. To visualize it, the data point above the red line will be removed.  
<img src="/images/cook_distance.png" width="300">  

## Results : 

Multiple Regression model is used without including **sex** as an independent variable after removing outliers.
Data is split into training and testing sets : 70% for training ; 30% for testing.

### Interpretation of the coefficients 
<img src="/images/summary_reg.png" width="500">  

- Age : when age increases by 1 unit, i.e. 1 year older, the insurance premium increase by $252.35, holding other variables constant
- Bmi : when bmi increases by 1 unit, the insurance premium increase by $294.96, holding other variables constant
- Children : when children increases by 1 unit, i.e. one more child, the insurance premium increase by $493.92, holding other variables constant
- Smoker : a smoker pays $24747.34 more than a non-smoker, holding other variables constant
- Region : 
  1) Northwest : Clients in Northwest pay $1003.73 less than Northeastern clients, holding other variables constant
  2) Southeast : Clients in Southeast pay $1125.22 less than Northeastern clients, holding other variables constant
  3) Southwest : Clients in Southeast pay $1094.49 less than Northeastern clients, holding other variables constant

### Regression Assumption Checking
#### Mean and normal distribution :
- Mean of residuals is 0
- The residuals are **not normally distributed** as P-value of Shapiro-Wilk test <5%. We reject H0: normal distribution.
- Since the dataset is **large enough** and the model is robust, we can still keep the model.

#### Autocorrelation and Multicollinearity :
- The residuals are **not autocorrelated** as P-value of Durbin Watson test is 9.2% which >5%. We accept H0: residuals are not autocorrelated. 
- All independent variables are not correlated as all VIF are <5.

#### Homoskedasticity :
- The residuals are heteroskedastic as P-value of Breusch-Pagan test is <5%. We reject H0: homoskedasticity. 
- Robust standard error is used to deal with Heteroskedasticity and get the adjusted standard error and t-test of the model.
Remedy:  
- Create a Covariance Matrix to calculate robust standard errors. 
- Use the robust standard errors to re-calculate a coefficient table as shown on the left.
- All independent variables are still significant but with different standard errors. 
- The estimated coefficients remain the same. Only t test becomes unreliable under heteroskedasticity.
### In-sample Analysis
<img src="/images/prediction.png" width="300">  
The model predicts most of the test data but not when the charges rise. There seems to be three groups of medical insurance charges. We may work further on it in the future.

## Acknowledgements :
[Lantz, B. (2013). Machine Learning with R, Packt Publishing.](https://www.amazon.com/Machine-Learning-R-Brett-Lantz/dp/1782162143)
	
