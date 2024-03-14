# Matplotlib-Challenge
### Pymaceuticals, Inc. - Squamous Cell Carcinoma Study
 
## Introduction
In the most recent animal study, 249 mice identified with squamous cell carcinoma (SCC) tumors were subjected to various drug regimens, with a primary focus on evaluating the efficacy of Pymaceuticals' leading drug, Capomulin. Over the span of 45 days, the development and measurement of tumor progression were meticulously observed. The objective of this comprehensive study was to assess the performance of Capomulin in comparison to other treatment regimens, providing valuable insights that can shape the future of anti-cancer medications.

This analysis uses statistical methods gain insight about each drugs performance. Quartiles, outliers, boxplots

## Imports
    # Dependencies and Setup
    import matplotlib.pyplot as plt
    import pandas as pd
    import scipy.stats as st
    from scipy.stats import linregress
    import numpy as np
## ETL 
Study data files

    mouse_metadata_path = "../data/Mouse_metadata.csv"
    study_results_path = "../data/Study_results.csv"

Read the mouse data and the study results
    
    mouse_metadata = pd.read_csv(mouse_metadata_path)
    study_results = pd.read_csv(study_results_path)

Combine the data into a single DataFrame
    
    tumor_df = pd.merge(study_results, mouse_metadata, how="left", on=["Mouse ID", "Mouse ID"])

Display the data table for preview

    tumor_df.head()

Check number of mice

![](Pymaceuticals/images/check_mouse_count.png)

Drop duplicates and confirm
    
    mouse_dup = tumor_df.loc[tumor_df.duplicated(subset=["Mouse ID", "Timepoint"]), "Mouse ID"].unique()

    Output: array(['g989'], dtype=object)

    cl_mouse_df = tumor_df[tumor_df["Mouse ID"] != "g989"]

Confirm duplicates are dropped

![](Pymaceuticals/images/clean_mouse_count.png)


## Analysis
1. Group by drug regimen 
2. Get statistic on Tumor Volume (mean,median, variance, standard deviation, standard error mean)
3. Export stats to ne df
4. Alternate statistical method - aggregation
## Results
### bar plots
1. list drugs
2. list timepoints
3. value count of drug regimen


4. Bar plot of Time points vs Drug regimen using Pandas
5. Bar plot of Time points vs Drug regimen using pylot
### Pie plots
6. value count of sex
7. Pie plot mouse ID vs sex using pandas
8. Pie plot mouse ID vs sex using pyplot

### Quartiles, Outliers, and boxplots
9. create df for all drug regimens
10. concatenate the treatment df
11. find the last timepoint for each mouse into df
12. merge treatment df and mouse timepoint df
13. filter using a boolean for the last time == 'true'
13. create summary df
14. use list comprehension with summary to analyze tumor volume per drug regimen and for outlier
15. generate boxplot for treatments and outlier
### Line and scatter plot
16. create line plot using a single mouse (b128) to identify tumor size ve timepoint
17. generate scatter plot of mouse weight vs average observed tumor
### Correlation and Linear Regression
18. generate correlation coefficient and linear regression model by plotting



## Conclusion


## Resources

 Find Mouse ID duplicates - How do I get a list of all the duplicate items using pandas in python? (Feb 20, 2013). StackOverflow. Website. URL. https://stackoverflow.com/questions/14657241/how-do-i-get-a-list-of-all-the-duplicate-items-using-pandas-in-python


 Grouping to find duplicate columns - Grouping by multiple columns to find duplicate rows pandas. (Oct 9, 2017). StackOverflow. Website. URL https://stackoverflow.com/questions/46640945/grouping-by-multiple-columns-to-find-duplicate-rows-pandas

#### Not equal filtering- Pandas: Filter by Column Not Equal to Specific Values. (Feb 10, 2023). Statology. Website. URL https://www.statology.org/pandas-filter-by-column-value-not-equal/#:~:text=Note%3A%20The%20symbol%20!%3D,%E2%80%9Cnot%20equal%E2%80%9D%20in%20pandas.

#### Merging dataframes - Merge, join, concatenate and compare. (2023). pandas. Website. URL https://pandas.pydata.org/docs/user_guide/merging.html

#### Concantenating dataframes - How To Concatenate Two or More Pandas DataFrames? (Nov 2. 2022). GeeksforGeeks. Website. URL https://www.geeksforgeeks.org/how-to-concatenate-two-or-more-pandas-dataframes/

#### Dropping rows using Boolean filter - How to Drop Pandas Rows Based on Index Condition (True/False). (Apr 2, 2023). StackOverflow. Webiste. URL https://stackoverflow.com/questions/66923323/how-to-drop-pandas-rows-based-on-index-condition-true-false

#### Formating plot - How to change outlier point symbol in Python matplotlib.pyplot. (Jan 9, 2021). StackOverflow. Website. URL https://stackoverflow.com/questions/65648502/how-to-change-outlier-point-symbol-in-python-matplotlib-pyplot

