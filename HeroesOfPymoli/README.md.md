

```python
##Andrew Kling
##UNCC HW 4 - Heroes of Pymoli

#Observable Trends:
#1. Male players make up a majority of the players in the game at roughly 84%
#2. The top age range for players are 20-24 years old making up approximately 45% of the player base
#3. The most profitable item is also the most purchase item, "Oathbreaker, Last Hope of the Breaking Storm"


import pandas as pd
import numpy as np
import os

inputFile = os.path.join("purchase_data.csv")
purchase_data = pd.read_csv(inputFile)
purchase_data.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase ID</th>
      <th>Age</th>
      <th>Item ID</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>780.000000</td>
      <td>780.000000</td>
      <td>780.000000</td>
      <td>780.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>389.500000</td>
      <td>22.714103</td>
      <td>92.114103</td>
      <td>3.050987</td>
    </tr>
    <tr>
      <th>std</th>
      <td>225.310896</td>
      <td>6.659444</td>
      <td>52.775943</td>
      <td>1.169549</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>7.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>194.750000</td>
      <td>20.000000</td>
      <td>48.000000</td>
      <td>1.980000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>389.500000</td>
      <td>22.000000</td>
      <td>93.000000</td>
      <td>3.150000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>584.250000</td>
      <td>25.000000</td>
      <td>139.000000</td>
      <td>4.080000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>779.000000</td>
      <td>45.000000</td>
      <td>183.000000</td>
      <td>4.990000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create dataframe that contains number of total unique players
total_players =len(purchase_data["SN"].unique())
total_players_df = pd.DataFrame({"Total Players":[len(purchase_data["SN"].unique())]})
total_players_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create Dataframe to show number of unique items, Average Price, Number of Purchases and Total Revenue
unique_items = purchase_data["Item Name"].nunique()
avg_price = purchase_data["Price"].mean()
total_purchases = purchase_data["Item Name"].size
total_revenue = purchase_data["Price"].sum()
purchases_df = pd.DataFrame({"Unique Items":[unique_items],"Average Price":avg_price,"Total Purchases":total_purchases,\
                    "Total Revenue":total_revenue})
#format price and revenue as currency
purchases_df["Average Price"] = purchases_df["Average Price"].map("${:.2f}".format)
purchases_df["Total Revenue"] = purchases_df["Total Revenue"].map("${:.2f}".format)
#arrange column order
purchases_df = purchases_df[["Unique Items","Average Price","Total Purchases","Total Revenue"]]
purchases_df

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unique Items</th>
      <th>Average Price</th>
      <th>Total Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2379.77</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create dataframe with each username only appearing once
unique_players_df = purchase_data.drop_duplicates(subset="SN")
#count of each gender
gender_count = unique_players_df["Gender"].value_counts()

male_count = int(gender_count[0])
female_count = int(gender_count[1])
nondisc_count = int(gender_count[2])

male_percent = male_count/total_players * 100
female_percent = female_count/total_players * 100
nondisc_percent = nondisc_count/total_players * 100

gender_df = pd.DataFrame({"Total Count":gender_count,"Percentage of Players":[male_percent,female_percent,nondisc_percent]})

gender_df["Percentage of Players"] = gender_df["Percentage of Players"].map("{:.2f}%".format)
gender_df


```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>84.03%</td>
      <td>484</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>14.06%</td>
      <td>81</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.91%</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#purchase count by gender
gender_purchase_count = purchase_data.groupby("Gender", as_index=False)[["SN"]].count()
#avg purchase price by gender
gender_avg_purchase_price = purchase_data.groupby("Gender",as_index=False)[["Price"]].mean()
#Total purchase value by gender
gender_total_purchase_price = purchase_data.groupby("Gender",as_index=False)[["Price"]].sum()


#TODO - NEED TO UPDATE NORMALIZED TOTALS WITH CORRECT CALCULATION*****
purchase_analysis_df =gender_purchase_count.merge(gender_avg_purchase_price, on="Gender")
purchase_analysis_df= purchase_analysis_df.merge(gender_total_purchase_price, on="Gender")
purchase_analysis_df = purchase_analysis_df.rename(columns={"SN":"Purchase Count","Price_x": "Average Purchase Price","Price_y":"Total Purchase Value"})
purchase_analysis_df["Normalized Totals"] = purchase_analysis_df["Total Purchase Value"] / purchase_analysis_df["Purchase Count"]

purchase_analysis_df["Average Purchase Price"] = purchase_analysis_df["Average Purchase Price"].map("${:.2f}".format)
purchase_analysis_df["Total Purchase Value"] = purchase_analysis_df["Total Purchase Value"].map("${:.2f}".format)
purchase_analysis_df["Normalized Totals"] = purchase_analysis_df["Normalized Totals"].map("${:.2f}".format)
purchase_analysis_df

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$3.20</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>652</td>
      <td>$3.02</td>
      <td>$1967.64</td>
      <td>$3.02</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$3.35</td>
    </tr>
  </tbody>
</table>
</div>




```python
#determine # of players and percentage of total players based on age range
age_bins = [0,9,14,19,24,29,34,39,200]
age_labels = ["<10","10-14","15-19","20-24","25-29","30-34","35-39","40+"]

#create new column for age range bin label
age_range_df = unique_players_df
age_range_df["Age Range"] = pd.cut(age_range_df["Age"],age_bins, labels = age_labels, include_lowest=True)

#groupby age range and determine sum and sum/total players
age_range_grouped_df = age_range_df.groupby("Age Range").count()
age_range_grouped_df = age_range_grouped_df[["Age"]]
age_range_grouped_df = age_range_grouped_df.rename(index=str, columns={"Age": "Total Count"})

#divide total count of player age by total amount of players to get percentage of player base
age_range_grouped_df["Percentage of Players"] = (age_range_grouped_df["Total Count"] / total_players * 100).map("{:.2f}%".format)
age_range_grouped_df = age_range_grouped_df.sort_index()
age_range_grouped_df
```

    C:\Users\kling\Anaconda3\lib\site-packages\ipykernel_launcher.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      import sys
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82%</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58%</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79%</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37%</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03%</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38%</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08%</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95%</td>
    </tr>
  </tbody>
</table>
</div>




```python
#sorted by age, list: purchase count, avg purchase price, total purchase value, normalized totals
age_purchase = purchase_data

age_purchase["Age Range"] =  pd.cut(age_purchase["Age"],age_bins, labels = age_labels, include_lowest=True)
age_purchase_grouped =  age_purchase.groupby("Age Range")
age_purchase_count = age_purchase_grouped["Purchase ID"].count()
age_purchase_avg = age_purchase_grouped["Price"].mean()
age_purchase_total = age_purchase_grouped["Price"].sum()

age_purchase_display = pd.DataFrame({"Purchase Count":age_purchase_count,
                                     "Average Purchase Price":age_purchase_avg.map("${:.2f}".format),
                                     "Total Purchase Value":age_purchase_total.map("${:.2f}".format),
                                     "Normalized Totals":age_purchase_avg.map("${:.2f}".format)})
age_purchase_display
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>$2.96</td>
      <td>$2.96</td>
      <td>28</td>
      <td>$82.78</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>$3.04</td>
      <td>$3.04</td>
      <td>136</td>
      <td>$412.89</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>$3.05</td>
      <td>$3.05</td>
      <td>365</td>
      <td>$1114.06</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>$2.90</td>
      <td>$2.90</td>
      <td>101</td>
      <td>$293.00</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>$2.93</td>
      <td>$2.93</td>
      <td>73</td>
      <td>$214.00</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>$3.60</td>
      <td>$3.60</td>
      <td>41</td>
      <td>$147.67</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>$2.94</td>
      <td>$2.94</td>
      <td>13</td>
      <td>$38.24</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>$3.35</td>
      <td>$3.35</td>
      <td>23</td>
      <td>$77.13</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Determine top 5 spenders by SN, get: purchase count, avg purchase price, total purchase value
top_spenders = purchase_data.groupby("SN")
top_spenders_avg = top_spenders["Price"].mean()
top_spenders_count = top_spenders["Purchase ID"].count()
top_spenders_total = top_spenders["Price"].sum()

total_spenders_display = pd.DataFrame({"Purchase Count":top_spenders_count,
                                       "Average Purchase Price":top_spenders_avg.map("${:.2f}".format),
                                       "Total Purchase Value":top_spenders_total.map("${:.2f}".format)})
total_spenders_display = total_spenders_display.sort_values(by=['Total Purchase Value'], ascending=False)
total_spenders_display = total_spenders_display.iloc[0:5,:]
total_spenders_display
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Haillyrgue51</th>
      <td>$3.17</td>
      <td>3</td>
      <td>$9.50</td>
    </tr>
    <tr>
      <th>Phistym51</th>
      <td>$4.75</td>
      <td>2</td>
      <td>$9.50</td>
    </tr>
    <tr>
      <th>Lamil79</th>
      <td>$4.64</td>
      <td>2</td>
      <td>$9.29</td>
    </tr>
    <tr>
      <th>Aina42</th>
      <td>$3.07</td>
      <td>3</td>
      <td>$9.22</td>
    </tr>
    <tr>
      <th>Saesrideu94</th>
      <td>$4.59</td>
      <td>2</td>
      <td>$9.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Determine top 5 most popular items by Item Name, get: purchase count, Item price, total purchase value
top_items = purchase_data.groupby("Item ID")
top_items_count = top_items["Purchase ID"].count()
top_items_price = top_items["Price"].mean()
top_items_total = top_items["Price"].sum()

top_items_display = pd.DataFrame({"Purchase Count":top_items_count,
                                 "Item Price":top_items_price,
                                 "Total Purchase Value":top_items_total})
#save copy to use in Most Profitable Items dataframe
item_profit_display = top_items_display

#reset index to add in item name
top_items_display = top_items_display.reset_index()
top_items_display= top_items_display.merge(purchase_data[["Item ID","Item Name"]], how="left", on="Item ID")
top_items_display = top_items_display.drop_duplicates()

#save copy to use in Most Profitable Items dataframe
item_profit_display = top_items_display

#retrieve only top 5 items
top_items_display = top_items_display.sort_values(by=["Purchase Count"], ascending=False)
top_items_display = top_items_display.iloc[0:5,:]

top_items_display = top_items_display.set_index("Item ID")   
top_items_display = top_items_display[["Item Name","Purchase Count","Item Price","Total Purchase Value"]]
top_items_display
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <td>Oathbreaker, Last Hope of the Breaking Storm</td>
      <td>12</td>
      <td>4.23</td>
      <td>50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <td>Fiery Glass Crusader</td>
      <td>9</td>
      <td>4.58</td>
      <td>41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>9</td>
      <td>3.53</td>
      <td>31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <td>Nirvana</td>
      <td>9</td>
      <td>4.90</td>
      <td>44.10</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Pursuit, Cudgel of Necromancy</td>
      <td>8</td>
      <td>1.02</td>
      <td>8.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
item_profit_display = item_profit_display.sort_values(by=['Total Purchase Value'], ascending=False)
item_profit_display = item_profit_display.iloc[0:5,:]
item_profit_display

#item_profit_display.set_index("Item ID")   
item_profit_display = item_profit_display[["Item Name","Purchase Count","Item Price","Total Purchase Value"]]
item_profit_display
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>750</th>
      <td>Oathbreaker, Last Hope of the Breaking Storm</td>
      <td>12</td>
      <td>4.23</td>
      <td>50.76</td>
    </tr>
    <tr>
      <th>340</th>
      <td>Nirvana</td>
      <td>9</td>
      <td>4.90</td>
      <td>44.10</td>
    </tr>
    <tr>
      <th>613</th>
      <td>Fiery Glass Crusader</td>
      <td>9</td>
      <td>4.58</td>
      <td>41.22</td>
    </tr>
    <tr>
      <th>381</th>
      <td>Final Critic</td>
      <td>8</td>
      <td>4.88</td>
      <td>39.04</td>
    </tr>
    <tr>
      <th>436</th>
      <td>Singed Scalpel</td>
      <td>8</td>
      <td>4.35</td>
      <td>34.80</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
