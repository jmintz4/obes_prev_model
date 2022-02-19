# obes_prev_model
Predicting the Prevalence of Obesity in U.S. Counties

# Overview and Problem Statement
What are the most important socioeconomic or food insecurity factors in predicting the prevalence of obesity in U.S. counties?  And how reliable are these factors in accurately predicting obesity prevalence?

For the study, demographic information such as income level and education is assembled with data on obesity and the "Food Environment Atlas".
* The Food Environment Atlas, from the USDA, contains data on factors such as store/restaurant proximity, food prices, food and nutrition assistance programs, and community characteristics, which interact to influence food choices and diet quality.
* County-level data on educational attainment is also sourced from the USDA.
* County-level measures of obesity prevalence are sourced from the Institute for Health Metrics and Evaluation (IHME).
* County-level data on poverty and income is sourced from the U.S. Census Bureau.

# Exploratory Data Analysis
The datasets are cleaned and then merged on the State/County or on the FIPS code.  In the Food Environment Atlas, high correlations exist due to:
* Fields being represented within both the overall population and a subset of that population, like seniors.
* Fields represented by both a count and a percentage of the population.
* Other items, such as farms, represented in a field as a general category, while more specific branches of those items are in other fields.

In the feature engineering stage:
* Removed one half of highly correlating, redundant pairs.
* Remove years not of interest, focusing in on 2010-2012.  From IHME, 2011 is the latest year with data on obesity prevalence.
* Combined some fields, e.g. create “overall” field from Male/Female data.

Percentages are kept over counts, and general fields are kept over their more specific branches.

After iterations of data cleaning, exploration, and feature engineering, the resulting data shape is:
* 48 variables 
    * 1 Target:  Obesity Prevalence by County from 2011
    * 47 Features
* 2166 counties

# Modeling and Results
Multiple regression models are compared and tuned in scikit-learn, evaluating both explanatory and predictive power.

![image](https://user-images.githubusercontent.com/91767180/154783606-b47c8e70-d248-43a3-8d84-974b0211af09.png)

* The Gradient Boosting Regressor (GBR) clearly has the best explanatory power (R<sup>2</sup>_train = 0.98)
* However, the GBR R<sup>2</sup>_train - R<sup>2</sup>_test difference indicates the model is not generalizable, i.e. the model is overfitting.
* Here, generalizability is more important than just a high explanatory power. 
* Even though the Ridge, Lasso, and Elastic Net have lower R<sup>2</sup> values, the R<sup>2</sup>_train - R<sup>2</sup>_test indicate these models are very generalizable.  Furthermore, 0.74 can still be considered a satisfactory explanatory and predictive power. 
* Among the Ridge, Lasso, and Elastic Net, the Lasso is slightly more generalizable. Lasso is therefore chosen as the best model.

Next, the feature importances are extracted, as interpreted from the Lasso coefficients.

Aside from the fields on education and percentage in poverty, variables from the Food Environment Atlas with high feature importance for predicting obesity prevalence include:
* FSRPTH11: Full-service restaurants/1,000 pop, 2011
* PERPOV10: Persistent-poverty counties, 2010
* PCT_LACCESS_HHNV10: Households, no car & low access to store (%), 2010
* SNAPSPTH12: SNAP-authorized stores/1,000 pop, 2012
* PCT_LOCLFARM12: Farms with direct sales (%), 2012
* PCT_SFSP12: Summer Food Service Program participants (% children), 2012
* PCT_REDUCED_LUNCH10: Students eligible for reduced-price lunch (%), 2010

# Conclusions and Next Steps
* Reliable indicators of obesity prevalence in a U.S. county:
    * Socioeconomic factors such as poverty and education level
    * Food insecurity factors, such as low access to affordable healthy food
* Lasso regression is chosen as the best model.
    * Satisfactory explanatory power
    * Ability to generalize well with new data
* Recommended next steps for potential model improvement:
    * Use other subsets of features after further eliminating high correlations / redundancies.
    * Test methods of filling missing data to retain more counties, protecting against sampling bias.
* Use Case:  Help local governments implement programs to target communities or groups that lack affordable healthy food options.
















