# Employment Growth and Office Property Values
<a id="data-source"></a>

Judy Chen

**Introduction** <br>
The COVID crisis and the large-scale Work-From-Home trend has introduced unprecedented uncertainty to the demand for office space. To provide some insight against it, this project aims to assess factors that have long-term impact on office real estate values beyond near-term economic crisis, one of which is the migration of major headquarters, which is generally believed to have a positive effect. The objective is to examine the job growth metrics, out of the following five, that have the most significant relationships with office property values: <br>

- TOT_EMP: Total employment <br>
- JOBS_1000: Number of jobs for an occupation per 1,000 jobs in a given area <br>
- LOC_QUOTIENT: Location quotient - ratio of an occupation’s share of employment in a given area to its share of employment in the U.S. as a whole <br>
- A_MEAN: Mean annual wage <br>
- A_MEDIAN: Annual median wage <br>
(See Data Dictionary for detail) <br>

E.g. do occupations that rise faster in number of employment have a more substantial effect on office real estate values than those that rise faster in salary? 

**Datasets** <br>
The feature variables are the ten-year annualized growth of five employment metrics from May 2010 to May 2020 in 11 Metropolitan Statistical Areas (MSA) of 23 occupations provided by Bureau of Labor and Statistics (BLS). The output variables, provided by the National Council of Real Estate Investment Fiduciaries (NCREIF) Property Index, are 10-year annualized total return, income return, and appreciation return of three categories of office properties: total office, CBD office and suburban office, in the same 11 MSAs as of 3/31/2021. The 10-month lag from the feature variables is employed intentionally to account for real estate values changing after economic indicators. <br>

Data Dictionary: <br>
https://github.com/jchen9619/Employment-Growth-and-Office-Property-Values/blob/main/Data%20Dictionary%20-%20Preprocessed%20Dataset.ipynb <br>

**Exploratory Data Analysis (EDA)** <br>
EDA Notebook: <br> 
https://github.com/jchen9619/Employment-Growth-and-Office-Property-Values/blob/master/EDA%20-%20Employment%20Growth%20and%20Office%20Property%20Values.ipynb

The following initial steps were performed to cleanse and aggregate the original datasets: <br>

Combine datasets and subset to only rows that contain data from both 2020 and 2010 <br>
Calculate annualized growth rate <br>
Filter data to appropriate granularity (e.g. Industry at the broadest level) <br>
Investigate and exclude missing and non-applicable values <br>

The major challenge in the EDA process is linking the BLS and NCREIF data at the same level of granularity. NCREIF data is provided at the MSA level and BLS is at MSA–occupation level. An adjustment factor is added to the NCREIF dataset to calculate the proportion of a specific occupation's average total employment between 2010 and 2020 in a given MSA. This assumes each occupation's contribution to the 10-year office real estate return is proportionate to the number of jobs for that occupation in the MSA. 

**Feature Engineering** <br>
Feature Engineering Notebook: <br>
https://github.com/jchen9619/Employment-Growth-and-Office-Property-Values/blob/master/Feature%20Engineering%20-%20Employment%20Growth%20and%20Office%20Property%20Values.ipynb

Since all five feature variables are either skewed to the left or right, and all Office returns are significantly left-skewed, I explored the following combinations of transformation on the underlying data: <br>
insert
- log transforming features <br>
- log transforming output variables <br>
- log transforming both features and output variables <br>
- square root transformation on features <br>
- square root transformation on output variables <br>
- square root transformation on both features and output variables <br>

The best model is produced when **both features and output variables are log-transformed**, as evidenced by the most significantly improved correlation between features and output variables. <br>

**Modeling** <br>
Modeling Notebook: <br>
https://github.com/jchen9619/Employment-Growth-and-Office-Property-Values/blob/master/Modeling%20-%20Employment%20Growth%20and%20Office%20Real%20Estate%20Values.ipynb

I fitted linear regression, Decision Tree and Random Forest on both the original and log-transformed dataset. For the latter two models, I also attempted hypermeter tuning and gradient-boosting (XGBoost and AdaBoost) to improve the output. In Linear Regression, R2 improved dramatically after applying log transformation to features and outpu variables, although at the expense of root-mean-squared-errors (RMSE) being significantly higher as well. However, with the other models, the log-transformation was not additive to lowering the errors. 

**The overall best model is Random Forest with untuned hyperparameters**, as it produced one of the lowest set of RMSEs and highest R2 scores (60-70%): 

  <img src="https://github.com/jchen9619/Employment-Growth-and-Office-Property-Values/blob/main/Agg_all_models.png" />
</p>

**Conclusion** <br>  
Occupations that have seen the fastest growth in **JOBS_1000** and **TOT_EMP** have the largest impact on accelerating the growth of office property values. The following occupations have experienced the highest 10-year annualized growth in these two metrics nationally. Corporate migrations in these industries have likely had the most impact on a MSA's office real estate value:

- Healthcare Support
- Transportation and Material Moving
- Management
- Computer and Mathematical
- Business and Financial Operations

**Assumptions and Limitations** <br>
- The adjustment factor that segmented original real estate returns from MSA level to MSA-occupation level assumes real estate returns are only driven by the break down of job types in an MSA, in reality, supply factors affect values significantly as well.
- If I had more time, I would add an additonal column stating whether the occupation if "H1B visa" dominant, meaning ranking in the top jobs nation wide that require the most H1B visas. I would be interested to see if jobs that have seen high growth in H1B sponsorship drove up office real estate value in those MSAs.
