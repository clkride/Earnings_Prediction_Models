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




<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Performance Evaluation

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



