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
Use groupby and summary statistical methods to calculate the following properties of each drug regimen: 
#mean, median, variance, standard deviation, and SEM of the tumor volume. 

    gr_drug = cl_mouse_df.groupby(["Drug Regimen"])
    gr_drug_mean = gr_drug["Tumor Volume (mm3)"].mean()
    gr_drug_mean = gr_drug["Tumor Volume (mm3)"].mean()
    gr_drug_median = gr_drug["Tumor Volume (mm3)"].median()
    gr_drug_var = gr_drug["Tumor Volume (mm3)"].var()
    gr_drug_std = gr_drug["Tumor Volume (mm3)"].std()
    gr_drug_sem = gr_drug["Tumor Volume (mm3)"].sem()

Assemble the resulting series into a single summary DataFrame. 

    gr_drug_df = pd.DataFrame({ "Mean Tumor Volume" : gr_drug_mean, 
                                "Median Tumor Volume" : gr_drug_median,
                                "Tumor Volume Variance" : gr_drug_var, 
                                "Tumor Volume Std. Dev." : gr_drug_std, 
                                "Tumor Volume Std. Err." : gr_drug_sem})

Alternate statistical method - aggregation produces summary statistics in a single line

    gr_drug_agg_df = cl_mouse_df[["Drug Regimen", "Tumor Volume (mm3)"]].groupby(["Drug Regimen"]).agg(["mean", "median", "var", "std", "sem"])

## Results
### Bar plots
#### **List drugs**

![](Pymaceuticals/images/list_drugs.png)

#### **List timepoints**

![](Pymaceuticals/images/list_timepoints.png)

#### **Value count of drug regimen**

![](Pymaceuticals/images/list_drug_regimen.png)

Bar plot of Time points vs Drug regimen using Pandas

    cl_mouse_df.groupby(["Drug Regimen"])["Timepoint"].count().reset_index(name="Timepoint").plot.bar(x="Drug Regimen",y="Timepoint",legend=False)
    plt.ylabel("# of Observed Mouse Timepoints")

 ![](Pymaceuticals/images/pandas_drugs.png)

Bar plot of Time points vs Drug regimen using pylot

    plot = cl_mouse_df.groupby(["Drug Regimen"])["Timepoint"].count().reset_index(name="Timepoint")
    tm_pt = plot["Timepoint"]
    x_axis = np.arange(len(tm_pt))
    dr_reg = plot["Drug Regimen"]

    plt.bar(x_axis,tm_pt,color="b")
    tick_locations = [x for x in x_axis]
    plt.xticks(tick_locations,dr_reg)
    #labels
    plt.xlabel("Drug Regimen")
    plt.ylabel("# of Observed Mouse Timepoints")
    plt.xticks(rotation="vertical")

 ![](Pymaceuticals/images/pyplot_drugs.png)

### Pie plots
#### **Value count of sex**

 ![](Pymaceuticals/images/sex_count.png)

Pie plot mouse ID vs sex using pandas

    cl_mouse_df.groupby(["Sex"])["Mouse ID"].count().plot.pie(y=["Sex"], label="",autopct="%1.1f%%")
    plt.ylabel("Sex")

 ![](Pymaceuticals/images/pandas_sex.png)

Pie plot mouse ID vs sex using pyplot

    pie_plot = cl_mouse_df.groupby(["Sex"])["Mouse ID"].count().reset_index(name = "Sex - Mouse ID")
    sx = pie_plot["Sex"]
    Count = pie_plot["Sex - Mouse ID"]
    plt.pie(Count, labels = sx, autopct = "%1.1f%%")
    plt.ylabel("Sex")

![](Pymaceuticals/images/pyplot_sex.png)

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

