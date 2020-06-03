
###  Overview

In this project, I have analyzed local and global temperature data and compared the temperature trends in Nashville, TN to overall global temperature trends. For this analysis, I have used the data provided in the Udacity portal.

### Goals

My goals for the project included creating a visualization and preparing a report describing the similarities and differences between global temperature trends and temperature trends in Nashville, TN. I started by extracting global temperature data as well as temperature data for Nashville through SQL queries. Then, I downloaded the CVS files. Using python, I created a line chart that compares the global and city temperatures, making sure to plot the moving average rather than the

yearly averages in order to obtain smooth lines and making trends more observable. Finally, I described the similarities and differences I observed from the data and also discussed overall trends. My project report includes an outline of the steps I took to prepare the data to be visualized in the chart, a line chart with global and city temperature trends, as well as some observations about the similarities and differences in the trends.

### Tools Used

**SQL:** To extract the data from the database  
**Python:** For calculating moving average and plotting the chart Jupyter Notebook: For writing python code 

### What I Did:

#### Step 1:
To see if Nashville was available in the database: 
```sql
SELECT * FROM city_list WHERE City = 'Nashville';
```

#### Step 2
To see what kind of information was available in city_data for Nashville: 
```sql
SELECT * FROM city_data WHERE city LIKE 'Nashville';
```

#### Step 3
To see what kind of information was given in global_data: 
```sql
SELECT * FROM global_data;
```

#### Step 4
To change the name of avg_temp column in city_data to avg_tempcity (so when I join the two tables, the name of average temperature columns will be different for the city and the world): 
```sql
ALTER TABLE city_data RENAME COLUMN avg_temp to avg_tempcity;
```

#### Step 5
To change the name of year column in city_data to yearcity (so when I join the two tables, the name of year columns will be different for the city and the world):  
```sql
ALTER TABLE city_data RENAME COLUMN year to year city;
```

#### Step 6
To join the average temperatures for Nashville and the world into one spreadsheet: 
```sql
SELECT global_data.year, global_data.avg_temp, city_data.avg_tempcity 
FROM global_data
JOIN city_data ON global_data.year = city_data.yearcity
WHERE city LIKE 'Nashville';
```

#### Step 7
 I exported the resulting spreadsheet from Step 5 to a `csv` file.

#### Step 6
I used Python for data visualization. Through my previous experience with Python, I knew about some libraries called `numpy`, `pandas` and `matplotlip`. So, I started by importing these onto Jupyter Notebook. I also imported the csv file from step 7. Then I calculated the moving average. All of this can be seen below.

```python
import numpy as np # for calculating the moving average
```


```python
import pandas as pd # for dealing with data
```


```python
from matplotlib import pyplot as plt # for plotting the data
```


```python
data = pd.read_csv("results.csv") # importing the csv file
```


```python
# calculating the moving average - source: 
# https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.core.window.Rolling.mean.html

moving_average_global = data["avg_temp"].rolling(35).mean()
moving_average_city = data["avg_tempcity"].rolling(35).mean()

```


```python
#plotting the data - source:
#https://swcarpentry.github.io/python-novice-gapminder/09-plotting/

plt.plot(data["year"], moving_average_city, label = "Nashville")
plt.plot(data["year"], moving_average_global, label = "Global")
plt. legend()
plt.xlabel("Years")
plt.ylabel("Temperature (oC)")
plt.title("Global Average Temperature")
plt. show()
```


![png](imgs/output_5_0.png)


### Conclusion:

1.  Nashville is around 5-6 degrees warmer than the global average. This difference is consistent over time.
    
2.  There is an overall increase in Nashville’s average temperature over time. This trend is also observed in the global average.
    
3.  The world seems to be getting hotter: From 1750 till the beginning of 1900s, the average temperature seems to be quite consistent (no major increase or decrease). However, from the begging of 1900s, there seems to be a trend for increase. Specifically, in the last 20 years, the rate of this increase seems to have gone up. This described pattern is seen for Nashville as well. This could be due to global warming.  
4. I observed that if I chose a short range for moving average (like 5 or 10), the lines were not smooth. If I chose a very long range (like 75 or 100), the lines were too smooth and it was harder to see fluctuations over shorter periods. So, I decided to present my data with a medium-sized range (35). This range gave smoother lines while also presenting the small fluctuations within shorter periods.
