

```python
print("Gender of most of the players is Male")
print("Maximum number of players are from the age group 20-24")
print("Total Purchase value shows significant increase with age and reaches the peak at agegroup 20-24, and decreases gradually with increase of age")
```

    Gender of most of the players is Male
    Maximum number of players are from the age group 20-24
    Total Purchase value shows significant increase with age and reaches the peak at agegroup 20-24, and decreases gradually with increase of age



```python
import json
import pandas as pd
```


```python
with open("Resources/purchase_data.json") as file:
    data = json.load(file)
```


```python
df=pd.DataFrame(data)
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Player Count
player_count = df.drop_duplicates().SN.value_counts().count()

df2 = pd.DataFrame({'Total Players':[player_count]})               
df2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Total)

#Number of unique items
unique_items = df["Item ID"].value_counts().count()

#Average Price
average_price = df["Price"].mean()

#Number of Purchases
purchases = df["Item ID"].count()

#Total Revenue
revenue = df["Price"].sum()

df3 = pd.DataFrame({'Number of Unique Items':[unique_items],
                 'Average Price':[average_price],
                 'Number of Purchases':[purchases],
                 'Total Revenue':[revenue]})
df3 = df3.round(2)
df3 ["Average Price"] = df3["Average Price"].map("${:,.2f}".format)
df3 ["Total Revenue"] = df3["Total Revenue"].map("${:,.2f}".format)
organized_df = df3[["Number of Unique Items","Average Price","Number of Purchases","Total Revenue"]]
organized_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics
total_gender_count = df["Gender"].value_counts()

player_gender_percent = total_gender_count/player_count*100

gender_demographics = pd.DataFrame({"Total Count": total_gender_count, "Percentage of Players": player_gender_percent})

gender_demographics = gender_demographics.round(2)
gender_demographics
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
      <td>110.47</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>23.73</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.92</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis Gender
gender_group = df.groupby("Gender")
gender_total = gender_group.sum()["Price"]
gender_average = gender_group.mean()["Price"]
gender_purchase_count = gender_group.count()["Price"]
normalized_total = gender_total / gender_demographics["Total Count"]

purchasing_analysis = pd.DataFrame({"Purchase Count": gender_purchase_count, "Average Purchase Price": gender_average, "Total Purchase Value": gender_total, "Normalized Totals": normalized_total})

purchasing_analysis["Average Purchase Price"] = purchasing_analysis["Average Purchase Price"].map("${:,.2f}".format)
purchasing_analysis["Total Purchase Value"] = purchasing_analysis["Total Purchase Value"].map("${:,.2f}".format)
purchasing_analysis["Normalized Totals"] = purchasing_analysis["Normalized Totals"].map("${:,.2f}".format)

purchasing_analysis = purchasing_analysis[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
purchasing_analysis
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$2.82</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$2.95</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$3.25</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Age Demographics
bins = [0, 9.99,14.99,19.99,24.99,29.99,34.99,39.99,200]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]
```


```python
age_binning = pd.cut(df["Age"], bins, labels=group_names)
```


```python
age_total = age_binning.value_counts()
age_percent = age_total/player_count*100
```


```python
age_demographics = pd.DataFrame({"Total Count": age_total, "Percentage of Players": age_percent})
age_demographics = age_demographics.round(2)
age_demographics.sort_index()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
      <th>&lt;10</th>
      <td>4.89</td>
      <td>28</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>6.11</td>
      <td>35</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>23.21</td>
      <td>133</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>58.64</td>
      <td>336</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>21.82</td>
      <td>125</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>11.17</td>
      <td>64</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>7.33</td>
      <td>42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>2.97</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Age) 
df["Age Ranges"] = pd.cut(df["Age"], bins, labels=group_names)
age_group = df.groupby(["Age Ranges"])

age_purchase_total = age_group.sum()["Price"]
age_average = age_group.mean()["Price"]
age_purchase_count = age_group.count()["Price"]
normalized_age_total = age_purchase_total / age_demographics["Total Count"]

age_purchasing_analysis = pd.DataFrame({"Purchase Count": age_purchase_count, "Average Purchase Price": age_average, "Total Purchase Value": age_purchase_total, "Normalized Totals": normalized_age_total})

age_purchasing_analysis["Average Purchase Price"] = age_purchasing_analysis["Average Purchase Price"].map("${:,.2f}".format)
age_purchasing_analysis["Total Purchase Value"] = age_purchasing_analysis["Total Purchase Value"].map("${:,.2f}".format)
age_purchasing_analysis["Normalized Totals"] = age_purchasing_analysis["Normalized Totals"].map("${:,.2f}".format)
age_purchasing_analysis = age_purchasing_analysis[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
age_purchasing_analysis
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
  </thead>
  <tbody>
    <tr>
      <th>10-14</th>
      <td>35</td>
      <td>$2.77</td>
      <td>$96.95</td>
      <td>$2.77</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$2.91</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$2.91</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$2.96</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$3.08</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$2.84</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$3.16</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>28</td>
      <td>$2.98</td>
      <td>$83.46</td>
      <td>$2.98</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders
spenders_group = df.groupby("SN")

spender_purchase_total = spenders_group.sum()["Price"]
spender_average = spenders_group.mean()["Price"]
spender_purchase_count = spenders_group.count()["Price"]

top_spenders = pd.DataFrame({"Total Purchase Value": spender_purchase_total, "Average Purchase Price": spender_average, "Purchase Count": spender_purchase_count})

top_spenders["Average Purchase Price"] = top_spenders["Average Purchase Price"].map("${:,.2f}".format)
top_spenders = top_spenders[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]
top_spenders = top_spenders.sort_values("Total Purchase Value", ascending=0)
top_spenders.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
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
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Items
popular_group = df.groupby(["Item ID","Item Name"])

item_purchase_total = popular_group.sum()["Price"]
item_average = popular_group.mean()["Price"]
item_purchase_count = popular_group.count()["Price"]

popular_items_demographics = pd.DataFrame({"Total Purchase Value": item_purchase_total, "Item Price": item_average, "Purchase Count": item_purchase_count})

popular_items_demographics["Item Price"] = popular_items_demographics["Item Price"].map("${:,.2f}".format)
popular_items_demographics["Total Purchase Value"] = popular_items_demographics["Total Purchase Value"].map("${:,.2f}".format)

popular_items_demographics = popular_items_demographics[["Purchase Count", "Item Price", "Total Purchase Value"]]

popular_items_demographics = popular_items_demographics.sort_values("Purchase Count", ascending=0)
popular_items_demographics.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable Items
profitable_items = popular_items_demographics.sort_values("Total Purchase Value", ascending=0)
profitable_items.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>170</th>
      <th>Shadowsteel</th>
      <td>5</td>
      <td>$1.98</td>
      <td>$9.90</td>
    </tr>
    <tr>
      <th>21</th>
      <th>Souleater</th>
      <td>3</td>
      <td>$3.27</td>
      <td>$9.81</td>
    </tr>
    <tr>
      <th>37</th>
      <th>Shadow Strike, Glory of Ending Hope</th>
      <td>5</td>
      <td>$1.93</td>
      <td>$9.65</td>
    </tr>
    <tr>
      <th>127</th>
      <th>Heartseeker, Reaver of Souls</th>
      <td>3</td>
      <td>$3.21</td>
      <td>$9.63</td>
    </tr>
    <tr>
      <th>120</th>
      <th>Agatha</th>
      <td>5</td>
      <td>$1.91</td>
      <td>$9.55</td>
    </tr>
  </tbody>
</table>
</div>


