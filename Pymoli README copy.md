
# Heroes of Pymoli Data Analysis 

<ul> 
<li> Although there are more male users than female users, on average, the purchase price is higher among females.  </li>
<li> The highest percentage of players are 20-24 years old. </li>
<li> The most popular item and the most profitable item is the Mourning Blade. </li>



```python
#Import Dependencies 
import pandas as pd
import numpy as np
```


```python
#Read in JSON file 
heroes_pymoli = "purchase_data2.json"
```


```python
heroes_pymoli_pd = pd.read_json(heroes_pymoli, encoding = "iso-8859-1")
heroes_pymoli_pd.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>Male</td>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>4.49</td>
      <td>Iloni35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>12</td>
      <td>Dawne</td>
      <td>3.36</td>
      <td>Aidaira26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17</td>
      <td>Male</td>
      <td>5</td>
      <td>Putrid Fan</td>
      <td>2.63</td>
      <td>Irim47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>Male</td>
      <td>123</td>
      <td>Twilight's Carver</td>
      <td>2.55</td>
      <td>Irith83</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>Male</td>
      <td>154</td>
      <td>Feral Katana</td>
      <td>4.11</td>
      <td>Philodil43</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Check column names 
heroes_pymoli_pd.columns
```




    Index(['Age', 'Gender', 'Item ID', 'Item Name', 'Price', 'SN'], dtype='object')




```python
#Drop Duplicates
heroes_without_duplicates = heroes_pymoli_pd.drop_duplicates(subset =['Age', 'Gender', 'SN'])
heroes_without_duplicates.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>Male</td>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>4.49</td>
      <td>Iloni35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>12</td>
      <td>Dawne</td>
      <td>3.36</td>
      <td>Aidaira26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17</td>
      <td>Male</td>
      <td>5</td>
      <td>Putrid Fan</td>
      <td>2.63</td>
      <td>Irim47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>Male</td>
      <td>123</td>
      <td>Twilight's Carver</td>
      <td>2.55</td>
      <td>Irith83</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>Male</td>
      <td>154</td>
      <td>Feral Katana</td>
      <td>4.11</td>
      <td>Philodil43</td>
    </tr>
  </tbody>
</table>
</div>



### Player Count


```python
#Player Count
#Total Players 
total_players = len(heroes_without_duplicates)
total_players
total_player_breakdown = pd.DataFrame({"Total Players": [total_players]})
total_player_breakdown
total_player_breakdown = total_player_breakdown[["Total Players"]]
total_player_breakdown
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
      <td>74</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Number of Unique Items
items_unique = heroes_pymoli_pd["Item Name"].nunique()
items_unique
```




    63




```python
#Avg Purchase Price
avg_price = heroes_pymoli_pd["Price"].mean()
avg_price
```




    2.9243589743589733




```python
#Total Number of Purchases
total_purchases = heroes_pymoli_pd["Price"].count()
total_purchases
```




    78




```python
total_price = heroes_pymoli_pd["Price"].sum()
total_revenue = avg_price*total_purchases 
total_revenue
```




    228.09999999999991



### Purchasing Analysis (Total)


```python
#Clean up the data/formatting
purchasing_analysis = pd.DataFrame({"Number of Unique Items": [items_unique],
                                   "Average Price": [avg_price], 
                                   "Number of Purchases": [total_purchases],
                                   "Total Revenue": [total_revenue]})
purchasing_analysis["Average Price"] = purchasing_analysis["Average Price"].map("${0:,.2f}".format)
purchasing_analysis["Total Revenue"] = purchasing_analysis["Total Revenue"].map("${0:,.2f}".format)
purchasing_analysis
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
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Number of Unique Items</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.92</td>
      <td>78</td>
      <td>63</td>
      <td>$228.10</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics
#Count of Male, Female and Other Players 
genders = heroes_without_duplicates["Gender"].unique()
genders
```




    array(['Male', 'Female', 'Other / Non-Disclosed'], dtype=object)




```python
total_male = heroes_without_duplicates["Gender"].value_counts()['Male']
total_female = heroes_without_duplicates["Gender"].value_counts()["Female"]
total_other = heroes_without_duplicates["Gender"].value_counts()["Other / Non-Disclosed"]
```


```python
#Percentage of Male, Female and Other Players
percentage_male = (total_male/total_players)*100
percentage_female = (total_female/total_players)*100
percentage_other = (total_other/total_players)*100
```

### Gender Demographics


```python
#Clean up/Format
gender_demographics = pd.DataFrame({"Percentage of Players": (percentage_male,percentage_female, percentage_other),
                                   "Total Count": (total_male, total_female,total_other)}) 

gender_demographics["Percentage of Players"] = gender_demographics["Percentage of Players"].map("{0:,.2f}".format)
gender_demographics
gender_demographics.rename(index={0:"Male",1:"Female",2:"Other / Non-Disclosed"})                               
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
      <td>81.08</td>
      <td>60</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.57</td>
      <td>13</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.35</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis by Gender
male_analysis = heroes_pymoli_pd.loc[heroes_pymoli_pd["Gender"] == "Male"]
male_analysis.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>Male</td>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>4.49</td>
      <td>Iloni35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>12</td>
      <td>Dawne</td>
      <td>3.36</td>
      <td>Aidaira26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17</td>
      <td>Male</td>
      <td>5</td>
      <td>Putrid Fan</td>
      <td>2.63</td>
      <td>Irim47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>Male</td>
      <td>123</td>
      <td>Twilight's Carver</td>
      <td>2.55</td>
      <td>Irith83</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>Male</td>
      <td>154</td>
      <td>Feral Katana</td>
      <td>4.11</td>
      <td>Philodil43</td>
    </tr>
  </tbody>
</table>
</div>




```python
male_purchase_count = male_analysis["Item Name"].count()
male_purchase_count
```




    64




```python
male_avg_price = male_analysis["Price"].mean()
male_avg_price
```




    2.884375




```python
male_purchase_value = male_analysis["Price"].sum()
male_purchase_value
```




    184.6




```python
normalized_male_value = male_purchase_value/total_male 
normalized_male_value

```




    3.0766666666666667




```python
#Female Purchasing Analysis
female_analysis = heroes_pymoli_pd.loc[heroes_pymoli_pd["Gender"] == "Female"]
female_purchase_count = female_analysis["Item Name"].count()
female_avg_price = female_analysis["Price"].mean()
female_purchase_value = female_analysis["Price"].sum()
normalized_female_value = female_purchase_value/total_female 

```


```python
#Other Purchasing Analysis 
other_analysis = heroes_pymoli_pd.loc[heroes_pymoli_pd["Gender"] == "Other / Non-Disclosed"]
other_purchase_count = other_analysis["Item Name"].count()
other_avg_price = other_analysis["Price"].mean()
other_purchase_value = other_analysis["Price"].sum()
normalized_other_value = other_purchase_value/total_other
```

### Purchasing Analysis (Gender)


```python
gender_purchasing_analysis = pd.DataFrame({"Purchase Count": (male_purchase_count,female_purchase_count,
                                other_purchase_count),
                                "Average Purchase Price": (male_avg_price, female_avg_price,
                                other_avg_price),
                                "Total Purchase Value": (male_purchase_value, female_purchase_value,
                                other_purchase_value),
                                "Normalized Totals":(normalized_male_value, normalized_female_value,
                                                             normalized_other_value)
            
                                          })

gender_purchasing_analysis
gender_purchasing_analysis["Average Purchase Price"] = gender_purchasing_analysis["Average Purchase Price"].map("${0:,.2f}".format)
gender_purchasing_analysis["Total Purchase Value"] = gender_purchasing_analysis["Total Purchase Value"].map("${0:,.2f}".format)
gender_purchasing_analysis["Normalized Totals"] = gender_purchasing_analysis["Normalized Totals"].map("${0:,.2f}".format)
gender_purchasing_analysis[['Purchase Count', 'Average Purchase Price', 'Total Purchase Value', 'Normalized Totals']]
gender_purchasing_analysis.rename(index={0:"Male",1:"Female",2:"Other / Non-Disclosed"})
gender_purchasing_analysis[['Purchase Count', 'Average Purchase Price', 'Total Purchase Value', 'Normalized Totals']]
gender_purchasing_analysis.rename(index={0:"Male",1:"Female",2:"Other / Non-Disclosed"})

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
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>$2.88</td>
      <td>$3.08</td>
      <td>64</td>
      <td>$184.60</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>$3.18</td>
      <td>$3.18</td>
      <td>13</td>
      <td>$41.38</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$2.12</td>
      <td>$2.12</td>
      <td>1</td>
      <td>$2.12</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics 
#Create the bins in which the Data will be held
print(heroes_pymoli_pd["Age"].max())
print(heroes_pymoli_pd["Age"].min())
```

    40
    7



```python
#Create Bins and Group Names
bins = [0,9,14,19,24,29,34,39,100]
group_names = ["<10", "10-14","15-19","20-24","25-29", "30-34", "35-39", "40+"]
#Cut Ages and place them into bins 
pd.cut(heroes_without_duplicates["Age"], bins, labels=group_names)
heroes_pymoli_pd["Age Bins"] = pd.cut(heroes_pymoli_pd["Age"], bins, labels=group_names)
heroes_pymoli_pd["Ages Binned"] = pd.cut(heroes_pymoli_pd["Age"],bins)
heroes_pymoli_pd.head()
#print without duplicates to find accurate age demographics 
without_dups = heroes_pymoli_pd.drop_duplicates(subset =['Age', 'Gender', 'SN'])
```


```python
age_group = without_dups.groupby("Age Bins")

heroes_pymoli_pd.head()
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>Age Bins</th>
      <th>Ages Binned</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20</td>
      <td>Male</td>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>4.49</td>
      <td>Iloni35</td>
      <td>20-24</td>
      <td>(19, 24]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>12</td>
      <td>Dawne</td>
      <td>3.36</td>
      <td>Aidaira26</td>
      <td>20-24</td>
      <td>(19, 24]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17</td>
      <td>Male</td>
      <td>5</td>
      <td>Putrid Fan</td>
      <td>2.63</td>
      <td>Irim47</td>
      <td>15-19</td>
      <td>(14, 19]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>Male</td>
      <td>123</td>
      <td>Twilight's Carver</td>
      <td>2.55</td>
      <td>Irith83</td>
      <td>15-19</td>
      <td>(14, 19]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>Male</td>
      <td>154</td>
      <td>Feral Katana</td>
      <td>4.11</td>
      <td>Philodil43</td>
      <td>20-24</td>
      <td>(19, 24]</td>
    </tr>
  </tbody>
</table>
</div>



### Age Demographics


```python
age_demographics = pd.DataFrame(without_dups["Age Bins"].value_counts())
age_demographics.reset_index(inplace=True)
age_demographics.columns = ["Age Bins", "Total Count"]
age_demographics
age_demographics["Percentage of Players"] = age_demographics["Total Count"] / total_players * 100
age_demographics["Percentage of Players"] = age_demographics["Percentage of Players"].map("{0:,.2f}".format)
age_demographics = age_demographics.sort_values(["Age Bins"], ascending=True)
age_demographics=age_demographics.set_index('Age Bins')
age_demographics
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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Age Bins</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5</td>
      <td>6.76</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3</td>
      <td>4.05</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>11</td>
      <td>14.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>34</td>
      <td>45.95</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>8</td>
      <td>10.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>6</td>
      <td>8.11</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>6</td>
      <td>8.11</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1</td>
      <td>1.35</td>
    </tr>
  </tbody>
</table>
</div>



### Purchasing Analysis (Age)


```python
age_purchasing_group = heroes_pymoli_pd.groupby("Age Bins")
age_analysis = pd.DataFrame(heroes_pymoli_pd["Age Bins"].value_counts())
age_analysis.reset_index(inplace=True)
age_analysis.columns = ["Age Bins", "Purchase Count"]
age_analysis = age_analysis.sort_values(["Age Bins"], ascending=True)
age_analysis=age_analysis.set_index('Age Bins')
price_bins = heroes_pymoli_pd.groupby("Age Bins").mean()
sum_bins = heroes_pymoli_pd.groupby("Age Bins").sum()
total_players = without_dups.groupby("Age Bins").count()
age_analysis["Average Purchase Price"] = price_bins["Price"]
age_analysis["Total Purchase Value"] = sum_bins["Price"]
age_analysis["Normalized Totals"] = age_analysis["Total Purchase Value"] / total_players["Item ID"]

age_analysis["Average Purchase Price"] = age_analysis["Average Purchase Price"].map("${0:,.2f}".format)
age_analysis["Total Purchase Value"] = age_analysis["Total Purchase Value"].map("${0:,.2f}".format)
age_analysis["Normalized Totals"] = age_analysis["Normalized Totals"].map("${0:,.2f}".format)
age_analysis

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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Bins</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5</td>
      <td>$2.76</td>
      <td>$13.82</td>
      <td>$2.76</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3</td>
      <td>$2.99</td>
      <td>$8.96</td>
      <td>$2.99</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>11</td>
      <td>$2.76</td>
      <td>$30.41</td>
      <td>$2.76</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>36</td>
      <td>$3.02</td>
      <td>$108.89</td>
      <td>$3.20</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9</td>
      <td>$2.90</td>
      <td>$26.11</td>
      <td>$3.26</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7</td>
      <td>$1.98</td>
      <td>$13.89</td>
      <td>$2.31</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>6</td>
      <td>$3.56</td>
      <td>$21.37</td>
      <td>$3.56</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1</td>
      <td>$4.65</td>
      <td>$4.65</td>
      <td>$4.65</td>
    </tr>
  </tbody>
</table>
</div>



### Top Spenders


```python
spending_analysis = pd.DataFrame(heroes_pymoli_pd["SN"].value_counts())
spending_analysis.reset_index(inplace=True)
spending_analysis.columns = ["SN", "Purchase Count"]
spending_analysis=spending_analysis.set_index('SN')
SN_price_bins = heroes_pymoli_pd.groupby("SN").mean()
SN_sum_bins = heroes_pymoli_pd.groupby("SN").sum()
spending_analysis["Average Purchase Price"] = SN_price_bins["Price"]
spending_analysis["Total Purchase Value"] = SN_sum_bins["Price"]
spending_analysis = spending_analysis.sort_values(["Total Purchase Value"], ascending=False)
spending_analysis["Average Purchase Price"] = spending_analysis["Average Purchase Price"].map("${0:,.2f}".format)
spending_analysis["Total Purchase Value"] = spending_analysis["Total Purchase Value"].map("${0:,.2f}".format)
spending_analysis.head()
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
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
      <th>Sundaky74</th>
      <td>2</td>
      <td>$3.71</td>
      <td>$7.41</td>
    </tr>
    <tr>
      <th>Aidaira26</th>
      <td>2</td>
      <td>$2.56</td>
      <td>$5.13</td>
    </tr>
    <tr>
      <th>Eusty71</th>
      <td>1</td>
      <td>$4.81</td>
      <td>$4.81</td>
    </tr>
    <tr>
      <th>Chanirra64</th>
      <td>1</td>
      <td>$4.78</td>
      <td>$4.78</td>
    </tr>
    <tr>
      <th>Alarap40</th>
      <td>1</td>
      <td>$4.71</td>
      <td>$4.71</td>
    </tr>
  </tbody>
</table>
</div>



### Most Popular Items


```python
item_popularity = pd.DataFrame(heroes_pymoli_pd["Item Name"].value_counts())
item_popularity.reset_index(inplace=True)
item_popularity.columns = ["Item Name", "Purchase Count"]
item_popularity=item_popularity.set_index('Item Name')
item_price_bins = heroes_pymoli_pd.groupby("Item Name").mean()
item_sum_bins = heroes_pymoli_pd.groupby("Item Name").sum()
item_popularity["Item Price"] = item_price_bins["Price"]
item_popularity["Total Purchase Value"] = item_sum_bins["Price"]
item_popularity = item_popularity.sort_values(["Purchase Count"], ascending=False)
item_popularity["Item Price"] = item_popularity["Item Price"].map("${0:,.2f}".format)
item_popularity["Total Purchase Value"] = item_popularity["Total Purchase Value"].map("${0:,.2f}".format)
item_popularity.head()
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
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Mourning Blade</th>
      <td>3</td>
      <td>$3.64</td>
      <td>$10.92</td>
    </tr>
    <tr>
      <th>Relentless Iron Skewer</th>
      <td>2</td>
      <td>$2.12</td>
      <td>$4.24</td>
    </tr>
    <tr>
      <th>Wolf</th>
      <td>2</td>
      <td>$2.70</td>
      <td>$5.40</td>
    </tr>
    <tr>
      <th>Heartstriker, Legacy of the Light</th>
      <td>2</td>
      <td>$4.71</td>
      <td>$9.42</td>
    </tr>
    <tr>
      <th>Crucifer</th>
      <td>2</td>
      <td>$2.65</td>
      <td>$5.29</td>
    </tr>
  </tbody>
</table>
</div>



### Most Profitable Items


```python
item_profitability = pd.DataFrame(heroes_pymoli_pd["Item Name"].value_counts())
item_profitability.reset_index(inplace=True)
item_profitability.columns = ["Item Name", "Purchase Count"]
item_profitability=item_profitability.set_index('Item Name')
item_price_bins2 = heroes_pymoli_pd.groupby("Item Name").mean()
item_sum_bins2 = heroes_pymoli_pd.groupby("Item Name").sum()
item_profitability["Item Price"] = item_price_bins2["Price"]
item_profitability["Total Purchase Value"] = item_sum_bins2["Price"]
item_profitability = item_profitability.sort_values(["Total Purchase Value"], ascending=False)
item_profitability["Item Price"] = item_profitability["Item Price"].map("${0:,.2f}".format)
item_profitability["Total Purchase Value"] = item_profitability["Total Purchase Value"].map("${0:,.2f}".format)
item_profitability.head()
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
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Mourning Blade</th>
      <td>3</td>
      <td>$3.64</td>
      <td>$10.92</td>
    </tr>
    <tr>
      <th>Heartstriker, Legacy of the Light</th>
      <td>2</td>
      <td>$4.71</td>
      <td>$9.42</td>
    </tr>
    <tr>
      <th>Apocalyptic Battlescythe</th>
      <td>2</td>
      <td>$4.49</td>
      <td>$8.98</td>
    </tr>
    <tr>
      <th>Betrayer</th>
      <td>2</td>
      <td>$4.12</td>
      <td>$8.24</td>
    </tr>
    <tr>
      <th>Feral Katana</th>
      <td>2</td>
      <td>$4.11</td>
      <td>$8.22</td>
    </tr>
  </tbody>
</table>
</div>


