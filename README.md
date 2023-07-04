<a name="readme-top"></a>
# Earnings_Prediction_Models
Used data from compustat and IBES (analyst forecasts) to implement three models on earnings prediction - HVZ, Earnings Persistence and Residual Income. Further, augmented these models with additional predictors to improve overall accuracy and predict top 10 firms that are most likely to exhibit largest earnings growth in FY'23.

## Table of Contents
- [About the Data Set](#about-data-set)
- [Feature Extraction](#feature-extraction)
- [Model Description](#model-description)
- [Performance Evaluation](#performance-evaluation)
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



