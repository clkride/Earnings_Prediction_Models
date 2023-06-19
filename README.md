# Earnings_Prediction_Models
Used data from compustat and IBES (analyst forecasts) to implement three models on earnings prediction - HVZ, Earnings Persistence and Residual Income. Further, augmented these models with additional predictors to improve overall accuracy and predict top 10 firms that are most likely to exhibit largest earnings growth in FY'23.

## Table of Contents
- [About the Data Set](#about-data-set)
- [Project Description](#project-description)
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



