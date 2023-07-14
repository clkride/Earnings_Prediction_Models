<a name="readme-top"></a>


![Bitbucket open issues](https://img.shields.io/github/issues/clkride/Earnings_Prediction_Models?style=flat-square)
<a href="https://linkedin.com/in/abbas-singapurwala">
<img src="https://img.shields.io/badge/LinkedIn-blue?style=flat&logo=linkedin&labelColor=blue">
</a>



# Earnings_Prediction_Models
Used data from compustat and IBES (analyst forecasts) to implement three models on earnings prediction - HVZ, Earnings Persistence and Residual Income. Further, augmented these models with additional predictors to improve overall accuracy and predict top 10 firms that are most likely to exhibit largest earnings growth in FY'23.

## Table of Contents
- [About the Data Set](#about-data-set)
- [Feature Extraction](#feature-extraction)
- [Model Description](#model-description)
- [Performance Evaluation](#performance-evaluation)
- [Conclusion](#conclusion)
- [Selecting Top 10 Stocks](#selecting-top-10-stocks)
- [Author](#author)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## About Data Set
In this project I have used the following datasets - 
 1. Compustat dataset - The first step was to download the Compustat dataset using the WRDS library. I have set the required filters, such as industry format, data format, population source, and consolidation level. Also set the time range from January 2009 to December 2023. Post that I merged the company data and the Compustat dataset to get the sic column, and a new column was created to store the first two digits of the SIC column.

2. IBES dataset - The next step is the task of downloading the financial data from the IBES database using the WRDS library. The data is filtered based on specific financial data format (EPS) and FPI equals to 1, which indicates actual data. The time range is set between January 2010 and December 2022. 

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Feature Extraction
This is a three step process - 
1. Caculating Financial metrics: the key metrics are summarized below for each financial model.
2. Estimating Changes in Financial Metrics: The changes in variables such as STDebt, CA, CL, Cash, and txp were calculated. The total current accruals (TCA) were calculated using these delta values.
3. Winsorizing the Dataset: Winsorizing the data is a statistical method used to mitigate the effect of outliers in a dataset. Outliers are values
that deviate significantly from other values in the dataset, and they can have a significant impact on statistical analysis and modeling.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Model Description
#### HVZ Model:
* 'at': Total Assets represents the total value of all assets that a company owns or controls. The variable 'at'
is a financial metric used to measure the size of a company.
* 'dvc': Debt to Value ratio is a financial metric used to assess the financial leverage of a company. It
measures the percentage of a company's total capital that is provided by debt. The variable 'dvc' represents
the ratio of the total amount of debt (both short-term and long-term) to the total market value of the company.
* 'E_t': Earnings is the net profit that a company generates during a particular period. It is the money that a
company has left over after paying all its expenses, taxes, and interest.
* 'dummy_Neg_E_t': It takes the value 1 if the earnings of the company are negative for a particular period,
and 0 otherwise. It is used to capture the effect of negative earnings on the stock price.
* 'ACT': Accounts Receivable Turnover is a financial ratio that measures how many times a company can
convert its accounts receivable into cash during a particular period. It is a measure of the effectiveness of
a company's credit and collection policies.

The regression coefficients are estimated using the ordinary least squares (OLS) method. The dependent variable
in the regression model is 'E_t1_HVZ', while the independent variables used in the model include 'at', 'dvc', 'E_t',
'dummy_Neg_E_t', and 'ACT'. The params_hvz_model_t1 variable is used to store the estimated coefficients of
the HVZ model.


#### EP Model:
* dummy_Neg_E_t': This is a dummy variable that takes the value 1 if the earnings surprise (EPS) for the
current fiscal quarter is negative and 0 otherwise.
* 'Neg_E_interaction_term': This variable is calculated as the product of 'dummy_Neg_E_t' and the change
in return on assets (ROA) from the previous year.
* EPS: Earning per share

The regression coefficients are estimated using the ordinary least squares (OLS) method. The dependent variable
in the regression model is 'E_t1_EP_RI', while the independent variables used in the model include
‘dummy_Neg_E_t', 'Neg_E_interaction_term', and 'EPS'. The params_EP_model_t1 variable is used to store the
estimated coefficients of the EP model.

#### RI Model:
* 'B_t': This variable represents the book value of equity (total shareholders' equity) at the end of the fiscal
quarter.
* 'TACC_t': This variable represents the tax-adjusted change in working capital (current assets - current
liabilities) from the previous year, divided by total assets at the end of the fiscal quarter. It is used as a
measure of cash flow management efficiency.

The regression coefficients are estimated using the ordinary least squares (OLS) method. The dependent variable
in the regression model is 'E_t1_EP_RI', while the independent variables used in the model include
'dummy_Neg_E_t', 'Neg_E_interaction_term', 'B_t', 'TACC_t'. The params_RI_model_t1 variable is used to store
the estimated coefficients of the RI model.


<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Performance Evaluation

The mean / median bias helps us in understanding if the model predictions are optimistic or pessimistic. 

* If the mean bias is positive, it means that the model has, on average, overestimated the actual values. 
* On the other hand, if the mean bias is negative, it means that the model has, on average, underestimated the actual values. 

#### For T+1 Model
* From the table, it can be seen that the mean bias of the HVZ model is significantly lower than that of the EP and RI
models
* The mean and median accuracy of the HVZ model is both higher than the corresponding measures of
the RI model

|&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;T+1 Model &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|
|---------------------------------------------------------------------------------------------------------------------|

|Metric| &nbsp; HVZ Model &nbsp; | &nbsp; EP Model &nbsp; | &nbsp; RI Model &nbsp;|
|:-------:|:---------:|:--------:|:--------:|
|Mean Bias <br /> (p value) | **180.26 <br /> (1.05e-34)** | -200.68 <br /> (3.47e-52) | -198.27 <br /> (2.77e-51)|
|Median Bias <br /> (p value) | **1.17 <br /> (2.79e-34)** | 1.17 <br /> (9.17e-53) | -0.78 <br /> (6.68e-51)|
|Mean Accuracy <br /> (p value) | **0.65 <br /> (0.9624)** | 1.19 <br /> (0.5033) | 0.79 <br /> (0.7555)|
|Median Accuracy <br /> (p value) | **1.12 <br /> (0.9491)** | 1.68 <br /> (0.0972) | 1.44 <br /> (0.3286)|

#### For T+2 Model
* The HVZ model has a significantly lower mean bias compared to the EP and RI models
* In terms of accuracy, the HVZ model has a similar or slightly lower mean accuracy compared to the EP and RI models

&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;T+2 Model &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|
|---------------------------------------------------------------------------------------------------------------------|

|Metric| &nbsp; HVZ Model &nbsp; | &nbsp; EP Model &nbsp; | &nbsp; RI Model &nbsp;|
|:-------:|:---------:|:--------:|:--------:|
|Mean Bias <br /> (p value) | **32.59 <br /> (0.0014)** | -367.61 <br /> (1.34e-77) | -341.36 <br /> (2.00e-68)|
|Median Bias <br /> (p value) | **0.37 <br /> (0.0012)** | 2.43 <br /> (1.43e-78) | -10.80 <br /> (2.09e-64)|
|Mean Accuracy <br /> (p value) | **0.60 <br /> (0.9634)** | 1.73 <br /> (0.4066) | 4.06 <br /> (0.5177)|
|Median Accuracy <br /> (p value) | **0.92 <br /> (0.9699)** | 0.87 <br /> (0.3309) | 0.84 <br /> (0.4967)|


#### For T+3 Model
* From the table, we can see that the RI model has the lowest mean and median bias compared to the other two models
* In terms of accuracy, the RI model has a lower mean bias compared to the HVZ model, although the difference is not statistically significant. However, the HVZ model has a negative mean accuracy, which indicates that its predictions are on average further from the actual values. The EP model also has a negative mean accuracy, although it is not statistically significant.

&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;T+3 Model &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|
|---------------------------------------------------------------------------------------------------------------------|

|Metric| &nbsp; HVZ Model &nbsp; | &nbsp; EP Model &nbsp; | &nbsp; RI Model &nbsp;|
|:-------:|:---------:|:--------:|:--------:|
|Mean Bias <br /> (p value) | 22.82 <br /> (0.122) | -462.25 <br /> (3.57e-56) | **-415.36 <br /> (6.61e-54)**|
|Median Bias <br /> (p value) | 2.17 <br /> (0.162) | 0.54 <br /> (2.57e-65) |**-16.71 <br /> (6.26e-50)**|
|Mean Accuracy <br /> (p value) | -46.45 <br /> (0.257) | -2.75 <br /> (0.3798) |**-26.09 <br /> (0.3076)**|
|Median Accuracy <br /> (p value) | 0.829 <br /> (0.259) | 0.43 <br /> (0.4553) |**0.57 <br /> (0.3153)**|


Once, we have selected one model each for t+1, t+2 and t+3, we’d now create these additional predictors to
augment our original models in an attempt to improve our model forecast accuracy. The additional predictors that
we’re going to add to each of these models are as follows:
* **Total Debt** - A financial metric that represents the sum of a company's long-term and short-term debt obligations.
* **Dividend payout ratio** - The proportion of a company's net income that is distributed to shareholders as dividends.
* **Sales growth** - A measure of the increase or decrease in a company's revenue over a specified period of time.
* **Return on assets** - A financial ratio that measures a company's profitability by comparing its net income to its total assets.
* **Debt-to-equity ratio** - A financial ratio that compares a company's total debt to its total equity. It is used to evaluate a company's leverage and risk.
* **Cash flow from operations** - The amount of cash generated or used by a company's operations, excluding financing, and investing activities.
* **Research and development expenses** - The costs associated with developing new products, processes, or technologies to maintain or improve a company's competitive position.

The summary statistics with modified approaches is summarized below for each t+1, t+2 and t+3 analysis.


&nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; |&nbsp; &nbsp; &nbsp; &nbsp;T+1 (HVZ Model) &nbsp; &nbsp; |&nbsp; &nbsp; &nbsp; T+2 (HVZ Model) &nbsp;  |&nbsp; &nbsp; &nbsp; &nbsp;T+3 (RI Model) &nbsp; &nbsp;&nbsp;&nbsp; |
|-------------------------|---------------------------------|------------------------------|-----------------------------|

|Metric| Previous | Modified | Previous | Modified | Previous | Modified |
|:-------:|:---------:|:--------:|:--------:|:---------:|:--------:|:--------:|
|R Square | 0.603 | **0.830** | 0.550 | **0.679** | 0.521 | **0.964** |
|Mean Bias <br /> (p value) | 180.26 <br /> (1.05e-34) | **98.96 <br /> (1.15e-05)** | 32.59 <br /> (0.0014)|**-160.88 <br /> (6.15e-10)** | -415.36 <br/> (6.61e-54) | **-722.00 <br/> (2.25e-25)**|
|Median Bias <br /> (p value) | 1.17 <br /> (2.79e-34) | **8.68 <br /> (6.29e-05)** | 0.37 <br /> (0.0012)| **16.45 <br/> (9.41e-12)**| -16.71 <br/> (6.26e-50)| **-27.80 <br/>(1.30e-23)**|
|Mean Accuracy <br /> (p value) | 0.65 <br /> (0.9624) | **1.47 <br /> (0.5850)** | 0.60 <br /> (0.9634)| **-0.81 <br/>(0.6816)**|-26.09 <br/> (0.3076)|**-2.23 <br/>(0.3436)**|
|Median Accuracy <br /> (p value) | 1.12<br/>(0.9491) |**0.95<br/>(0.5487)** |0.92<br/>(0.9699)|**0.82<br/>(0.7111)**|0.57<br/>(0.3153)|**0.15<br/>(0.4842)|

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Conclusion
* For **T+1 model**, the addition of predictors to original HVZ model improves R-squared value from 0.603 to 0.830 and decreases the mean bias by almost 50% from 180.26 to 98.96, indicating that it is a better model than the previous one.
* For **T+2 model**, the modified HVZ model with extra predictors performs better than the previous approach in terms of R-squared, median bias, and median accuracy. 
* For **T+3 model**, the modified RI model is better then the previous model because it results in a significantly higher R-squared value, indicating a better fit between the model and the data. Additionally, the mean and median biases have improved significantly, with the mean bias changing from a highly negative value to a much smaller positive value. 

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Selecting Top 10 Stocks

**Earnings growth and the top 10 firms:**
Earnings growth refers to the rate at which a company's earnings increase over time. A positive earnings growth rate means that a company's earnings have increased, while a negative growth rate means that earnings have decreased.

Earnings growth calculation: (predicted_earnings_2023 - E_t) / E_t

Unit: The unit of earnings growth is typically a percentage or a ratio.

* predicted_earnings_2023: the earnings predicted by the model for 2023
* E_t: the actual earnings in 2022

The top 10 firms predicted to have the largest earnings growth in 2023 are as follows:

|GVKEY| Predicted Earnings Growth | Company |
|:-------:|:---------:|:--------:|
| 016564 | 8877.49 | Wall Street Media Co |
| 001718 | 3031.38 | Advanced Oxygen Technology |
| 175548 | 927.86 | Nexgenrx Inc |
| 017349 | 337.16 | Skkynet Cloud Systems Inc |
| 024368 | 125.69 | Retail Holdings NV |
| 021563 | 108.65 | Electronic System Tech Inc |
| 060801 | 105.98 | Socket Mobile Inc |
| 143788 | 102.29 | Reflect Scientific Inc |
| 001783 | 97.65 | Arts Way MFG Inc|
| 011372 | 80.21 | New Concept Energy Inc|

 
<p align="right">(<a href="#readme-top">back to top</a>)</p>


## Author
 @[Abbas S.](https://github.com/clkride)

## License
The MIT License (MIT)

Copyright (c) 2023 Abbas Singapurwala

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Acknowledgments
Inspiration, code snippets, etc.
* [Choose an Open Source License](https://choosealicense.com)
* [Img Shields](https://shields.io)
* [GitHub Pages](https://pages.github.com)
<p align="right">(<a href="#readme-top">back to top</a>)</p>



