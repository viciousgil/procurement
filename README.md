# Spend Analytics
This project analyzes spend data for public projects by a government entity, identifying what areas have the highest and lowest budget and also visualizing the trend in spend towards the end of the project

### Data Sources
[Data World](https://data.world/city-of-ny/n7gv-k5yt)

### Tools
- SQL - Querying the database
- Python - Cleaning the dataset/Exploratory data analysis
- Power BI - Creating/Visualizing report

### Getting the data
In order to get the data, we queried data.org, a database using SQL. This is shown below
```sql
SELECT date_reported_as_of as project_name, category, budget_forecast, latest_budget_changes, total_budget_changes
 FROM capital_projects_1;
```
### Data Cleaning/Preparation
In the initial data preparation phase, we performed the following tasks:
1. Data loading into Jupyter Notebooks
   ```python
   import pandas as pd
   import matplotlib.pyplot as plt
   import seaborn as sns
   df = pd.read_csv(r"C:\Users\dell\Desktop\Study\Capital_Projects.csv")
   print(df.head(10))
   ```

2. Handling missing values using pandas
   ```python
      df2 = df1.copy()
      nan_values = df2["latest_budget_changes"].isna().sum()
      print("The number of NaN values in this columns is:", nan_values)
      df2["latest_budget_changes"].fillna(0, inplace = True)
   ```
3. Data cleaning and formatting

### Exploratory Data Analysis
EDA involved exploring procurement data to answer key questions like:
1. What project category has the highest and lowest budget forecast?
  ```python
     grouped = df2.groupby("category")["budget_forecast"].mean().sort_values(ascending=True)
     plt.figure(figsize=(10,6))
     sns.barplot(x= grouped.values, y = grouped.index, palette = "viridis")
     plt.title("Mean budget forecast by category")
     plt.xlabel("Mean budget forecast")
     plt.ylabel("Category")
     plt.show
  ```
2. What is the average budget per category?
   ![image](https://github.com/viciousgil/procurement/assets/139291982/0075d06c-9389-4a72-ad64-64faed95dc22)

3. What project category had the highest and lowest average spend at the end of the project?
 ```python
    # We want to add budget forecast rows and total budget changes rows to give us a column that shows the current budget
    def add_columns (row):
      return row["budget_forecast"]+row["total_budget_changes"]
      df2["Current Budget"] = df2.apply(add_columns, axis =1)
      df2.insert(df2.columns.get_loc("latest_budget_forecast")+1, "Current Budget", df2["Current Budget"])
```
-- A new table was also created in Power BI using DAX
### Results/Findings
![image](https://github.com/viciousgil/procurement/assets/139291982/5f059f52-1370-4b19-b1fa-b977af959a5e)
- From the interactive BI visuals above, we can see that Wastewater treatment had the highest budget and spend at the end of the projects
- The total cost for the projects was underestimated as an additional $23bn in total was spent to meet the cost of the projects
- Wastewater treatment had the highest underestimation, followed by Water Supply

### Recommendations
- More funds should be put aside towards Wastewater treatment and Water Supply projects in the future to account for underestimated costs
- Factors that led to the increased spend by $23bn should be investigated and accounted for to increase the accuracy of future predictions

### Limitations
- I had to remove the null values in Power BI (using Power Query) again as technical issues caused the cleaned data in python not to export to Power BI
- I also had to recreate the Total Spend Column in Power BI using DAX as I was unable to export the cleaned data from Jupyter Notebooks (will improve my learning on this)
