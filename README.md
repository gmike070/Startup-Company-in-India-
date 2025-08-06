# Startup-Company-in-India-
Using Python to analysis Startup companies in India Ecosystem
### Excited to share my latest project!
I recently conducted a data analysis on startup companies in India using Python. The analysis revealed some fascinating insights, and I've created some compelling visualizations to showcase the findings.

### Analyzing the growth of Indian startups Problem statement:
1.	How the ecosystem has changed with time
2.	How the numbers of startup have increased
3.	The support and funding option that is available
   
The dataset comprises 3000 observations and 10 attribute

### Setting up the environment
Installing the required libraries
We need Numpy for mathematical operation
Pandas for Dataframe Manipulation
Seaborn and Matplotlib for data visualization
And finally Jupyter Notebook to build an interactive ipython notebook

### Data Cleaning
Data cleaning is the process of detecting and correcting or removing the corrupt or inaccurate records for a record set, dataframe, database and refers to identifying the incomplete, incorrect and inaccurate or irrelevant parts of the data and then replacing, modifying and deleting the dirty or coarse data.
Again, Data cleaning is crucial and emphasized because wrong data can drive a business to wrong decisions, conclusion and pool analysis especially if the huge quantities of big data are into the picture.

### ANALYZING THE GROWTH OF INDIAN STARTUPS
### ANALYSIS OBJECTIVES:
1. How the ecosystem has changed with time
We’ll track how industries, cities, and sectors have evolved over the years.
2. How the number of startups has increased
We'll calculate startups founded or funded per year.
3. The support and funding options that are available
We'll identify top investors, investment types, and funding trends.

### PYTHON CODE (Step-by-Step)
Step 1: Import Libraries
   ```python
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns
 ```
Step 2: Load and Inspect the Dataset
Python:
 ```python
  import pandas as pd
  data = pd.read_csv("C:\\Users\\Admin\\Desktop\\DATA ANALITIC WITH PYTHON\\11. StartUps Case Study And Analysis\\startup_funding.csv")
  # changing the names of the columns inside the daata
  data.columns = ["SNo", "Date", "StartupName", "IndustryVertical", "Subvertical", "City", "InvestorsName", "InvestmentType", "AmountInUSD", "Remarks"]
  # lets clean the strings
  def clean_string(x):
    return str(x).replace("\\xc2\\xa0", "").replace("\\\\xc2\\\\xa0", "")
  # lets apply the function to clean the data
  for col in ["SNo", "StartupName", "IndustryVertical", "Subvertical", "City", "InvestorsName", "InvestmentType", "AmountInUSD", "Remarks"]:
    data[col]= data[col].apply(lambda x: clean_string(x))
       
  # lets check the head of the data    

   data.head()
 ```
Output:
 <img width="975" height="247" alt="image" src="https://github.com/user-attachments/assets/b452021c-d02f-44d5-b72b-345b80ac13b2" />

Step 3: Data Cleaning & Preprocessing
Python code:
```python
 # DATA CLEANING

# missing data
# lets import warnings module

import warnings
warnings.filterwarnings('ignore')
# lets calculate the total missing values
total = data.isnull().sum().sort_values(ascending = False)
# lets calculate the percentage of missing values in the data
percent = ((data.isnull().sum()/data.isnull().count())*100).sort_values(ascending = False)
# lets store the above two values in a dataset called missing data
missing_data = pd.concat([total, percent], axis=1, keys=['total', 'percent %'])

# lets check the head of the data
missing_data
```
```python
# Convert Date column to datetime format
data['Date'] = pd.to_datetime(data['Date'], errors='coerce')

# Drop rows with null dates
data = data.dropna(subset=['Date'])

# Extract year and month
data['Year'] = data['Date'].dt.year
data['Month'] = data['Date'].dt.month

# Clean Amount column (remove commas, '+', and NaNs)
data['AmountInUSD'] = data['AmountInUSD'].replace('[\$,]', '', regex=True)
data['AmountInUSD'] = pd.to_numeric(data['AmountInUSD'], errors='coerce')
```
### ANALYSIS 1: Ecosystem Change Over Time
a. Industry Growth Over Years
Python code:
 ```python
industry_by_year = data.groupby(['Year', 'IndustryVertical']).size().unstack().fillna(0)

industry_by_year.plot(kind='area', stacked=True, figsize=(14,7))
plt.title("Industry Vertical Distribution Over the Years")
plt.xlabel("Year")
plt.ylabel("Number of Startups")
plt.legend(loc='upper left', bbox_to_anchor=(1, 1))
plt.tight_layout()
plt.show()
 ``` 
Output:
 <img width="975" height="376" alt="image" src="https://github.com/user-attachments/assets/383c9ec7-1bae-4890-856f-22555fc0ad43" />



b. City-wise Activity Over Time
Python code:
```python city_year = data.groupby(['Year', 'City']).size().unstack().fillna(0)
top_cities = city_year.sum().sort_values(ascending=False).head(5).index
city_year[top_cities].plot(figsize=(12,6))
plt.title('Top Startup Cities Over the Years')
plt.ylabel('Number of Startups')
plt.xlabel('Year')
plt.show()
``` 
Output:
<img width="825" height="429" alt="image" src="https://github.com/user-attachments/assets/93328a62-3a54-4927-a3f1-10eeb2678f07" />

### ANALYSIS 2: Startup Growth
a. Number of Startups Funded Per Year
Python code:
 ```python
startup_growth = data.groupby('Year')['StartupName'].nunique()

startup_growth.plot(kind='bar', figsize=(10,6))
plt.title('Unique Startups Funded Per Year')
plt.xlabel('Year')
plt.ylabel('Number of Startups')
plt.show()
 ```
Output:

<img width="647" height="408" alt="image" src="https://github.com/user-attachments/assets/2d40e2dc-675e-405d-92ff-421ef6b4977e" />
 

b. Cumulative Growth
Python code:
```python
 cumulative = data.groupby('Year')['StartupName'].nunique().cumsum()

cumulative.plot(marker='o', figsize=(10,6))
plt.title('Cumulative Startup Growth')
plt.xlabel('Year')
plt.ylabel('Total Unique Startups')
plt.grid(True)
plt.show()
```
Output:

 <img width="609" height="368" alt="image" src="https://github.com/user-attachments/assets/e2cdcde4-cc3f-4ce4-86fc-134dc858bf47" />

### ANALYSIS 3: Support & Funding
a. Most Active Investors
Python code:
 
Output:
 
b. Funding Amount Over the Years
Python code:
```python
top_investors = data['InvestorsName'].str.split(',').explode().str.strip().value_counts().head(10)

sns.barplot(x=top_investors.values, y=top_investors.index)
plt.title("Top 10 Most Active Investors")
plt.xlabel("Number of Investments")
plt.ylabel("Investor Name")
plt.show()
 ```
Output:

 <img width="975" height="220" alt="image" src="https://github.com/user-attachments/assets/8c0500bc-9d19-40d3-bd4c-8ca606a926b5" />

c. Who plays the main role in indian startups Ecosytems?
Python Code:
  ```python
funding_by_year = data.groupby('Year')['AmountInUSD'].sum()

funding_by_year.plot(kind='bar', figsize=(10,6))
plt.title('Total Funding Amount Per Year (USD)')
plt.xlabel('Year')
plt.ylabel('Amount in USD')
plt.show()
 ```
Output:

 <img width="650" height="443" alt="image" src="https://github.com/user-attachments/assets/57d5eb08-06f2-444c-b058-91ca2fcd1dbf" />

### Recommendations & Conclusion
✅ 1. How the Ecosystem Has Changed Over Time
Findings:

The Indian startup ecosystem has matured significantly from 2015 onwards, with a surge in funding between 2015–2019 and in some cases post-2021.

Sectoral focus has shifted:

From early dominance of E-Commerce, Travel, and Marketplace platforms

To newer areas like FinTech, HealthTech, EdTech, and AI-based platforms

Cities like Bangalore, Mumbai, and Delhi NCR remain dominant hubs, but there’s visible growth in Tier-2 cities like Pune, Ahmedabad, and Jaipur.

Recommendation:

Investors and policymakers should foster decentralized innovation by supporting startup incubation in Tier-2 and Tier-3 cities.

Encourage industry-specific accelerators in emerging verticals like Climate Tech and Agritech.

✅ 2. How the Number of Startups Has Increased
Findings:

There has been a steady year-on-year growth in the number of unique startups receiving funding.

Cumulative growth shows that Indian startups are increasingly attracting capital and market attention.

The post-pandemic recovery (2021) saw renewed investor interest.

Recommendation:

Continue nurturing early-stage funding environments (e.g., angel networks, startup seed funds).

Provide structured mentorship through government or industry-led programs to support early-stage scalability.

✅ 3. The Support and Funding Options That Are Available
Findings:

Angel investments, Seed funding, and Series A dominate early years.

Later stages (Series B & C) are more selective and sector-focused.

Top investors include venture capital firms like Sequoia Capital, Accel Partners, and SAIF Partners.

Government schemes (like Startup India) are present but underrepresented in disclosed investor lists.

Recommendation:

Create more accessible platforms for startups to find suitable investors based on their stage and sector.

Improve awareness and reach of government grants and equity-free funding opportunities.



### Overall Conclusion
The data reflects a vibrant and rapidly evolving startup ecosystem in India, driven by both private and public funding. With increased urban-rural connectivity, digital penetration, and global investor interest, India is on track to becoming a global innovation powerhouse.
To maintain this momentum:
•	Strategic funding, regional inclusion, and sector-specific mentoring are crucial.
•	Government bodies, educational institutions, and investors should collaborate to build a sustainable and inclusive startup culture.

