

```python
## Dependencies
import pandas as pd
```


```python
data_file = "purchase_data.json"
data_file_df = pd.read_json(data_file)
data_file_df.head()
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
player_count = data_file_df["SN"].nunique()
player_count_df = pd.DataFrame({"Total Players": [player_count]})
player_count_df
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
#Purchase Analysis 
#Num of Unique Items
types = len(data_file_df["Item Name"].unique().tolist())

#Avg Purchase Price
avg_price = data_file_df["Price"].mean()

#Total Rev
total_rev = data_file_df["Price"].sum()

#Total Num of Purchases
total_purc = data_file_df["Item ID"].count()

#Create DataFrame
purchase_analysis = pd.DataFrame({"Number of Unique Items": [types], "Average Price": [avg_price], 
                                  "Total Revenue": [total_rev], "Total Purchases": [total_purc]})
#Format
purchase_analysis["Average Price"] = purchase_analysis["Average Price"].map("${:.2f}".format)
purchase_analysis["Total Revenue"] = purchase_analysis["Total Revenue"].map("${:,.2f}".format)

#Display Data
purchase_analysis
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
      <th>Average Price</th>
      <th>Number of Unique Items</th>
      <th>Total Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>179</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Demographics 
gender_data = data_file_df["Gender"].value_counts()
play_perc = (gender_data/total_purc)*100

#Create new DataFrame
play_perc_df = pd.DataFrame({"Percent of Players": play_perc.round(2), "Total Count": gender_data})

#Display Data
play_perc_df
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
      <th>Percent of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.44</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.41</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Gender Analysis
gender_ana = data_file_df.groupby(["Gender"])

gender_purc = gender_ana["Item ID"].count()
gender_avg = gender_ana["Price"].mean()
gender_rev = gender_ana["Price"].sum()
norm_total = (gender_rev / total_purc)

#Create DataFrame
gender_analysis = pd.DataFrame({"Purchase Count": gender_purc, "Average Price": gender_avg, 
                              "Total Revenue": gender_rev, "Normalized Data": norm_total.round(2)})

#Formate
gender_analysis["Average Price"] = gender_analysis["Average Price"].map("${:.2f}".format)
gender_analysis["Total Revenue"] = gender_analysis["Total Revenue"].map("${:,.2f}".format)

#Display
gender_analysis
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
      <th>Average Price</th>
      <th>Normalized Data</th>
      <th>Purchase Count</th>
      <th>Total Revenue</th>
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
      <td>$2.82</td>
      <td>0.49</td>
      <td>136</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>2.39</td>
      <td>633</td>
      <td>$1,867.68</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>0.05</td>
      <td>11</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographic 
bins = [0, 10, 14, 19, 23, 27, 31, 35, 39]
labels = ["<10", "10-14", "15-19", "19-23", "23-27", "27-31", "35-39", "40+"]

data_file_df["Age Demographic"] = pd.cut(data_file_df["Age"], bins, labels=labels)
data_file_df.head()

age_df = data_file_df.groupby(["Age Demographic"])

#Analysis
age_df_mean = age_df["Price"].mean()
age_df_sum = age_df["Price"].sum()
age_df_count = age_df["Item ID"].count()
norm_age = age_df_sum / total_purc

#Create new Dataframe
age_data = pd.DataFrame({"Avg. Purchase Price": age_df_mean,
                       "Total Purchase Value": age_df_sum,"Purchase Count": age_df_count,
                        "Normalized Total": norm_age.round(2)})

#Formate
age_data["Avg. Purchase Price"] = age_data["Avg. Purchase Price"].map("${:.2f}".format)
age_data["Total Purchase Value"] = age_data["Total Purchase Value"].map("${:.2f}".format)

#Display                                                                      
age_data
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
      <th>Avg. Purchase Price</th>
      <th>Normalized Total</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Age Demographic</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>$3.02</td>
      <td>0.12</td>
      <td>32</td>
      <td>$96.62</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>$2.70</td>
      <td>0.11</td>
      <td>31</td>
      <td>$83.79</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>$2.91</td>
      <td>0.50</td>
      <td>133</td>
      <td>$386.42</td>
    </tr>
    <tr>
      <th>19-23</th>
      <td>$2.88</td>
      <td>0.98</td>
      <td>266</td>
      <td>$765.31</td>
    </tr>
    <tr>
      <th>23-27</th>
      <td>$3.02</td>
      <td>0.65</td>
      <td>169</td>
      <td>$510.02</td>
    </tr>
    <tr>
      <th>27-31</th>
      <td>$2.96</td>
      <td>0.23</td>
      <td>60</td>
      <td>$177.40</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>$3.11</td>
      <td>0.17</td>
      <td>42</td>
      <td>$130.64</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>$2.75</td>
      <td>0.11</td>
      <td>30</td>
      <td>$82.38</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top Spenders
spender_total = data_file_df.groupby(['SN'])
spender_sum = pd.DataFrame(spender_total.sum()["Price"])
top_spenders = spender_sum.sort_values("Price", ascending=False)

#Spender Count & Mean
spender_count = data_file_df.groupby(['SN']).count()
spender_mean = spender_total["Price"].mean()

#Add Columns
top_spenders["Avg. Purchase Price"] = spender_mean
top_spenders["Purchase Count"] = spender_count["Item ID"]

#Format
top_spenders["Avg. Purchase Price"] = top_spenders["Avg. Purchase Price"].map("${:.2f}".format)
top_spenders["Price"] = top_spenders["Price"].map("${:.2f}".format)

#Display top 5
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
      <th>Price</th>
      <th>Avg. Purchase Price</th>
      <th>Purchase Count</th>
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
      <td>$17.06</td>
      <td>$3.41</td>
      <td>5</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>$13.56</td>
      <td>$3.39</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>$12.74</td>
      <td>$3.18</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>$12.73</td>
      <td>$4.24</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>$11.58</td>
      <td>$3.86</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular
popular_total = data_file_df.groupby(['Item Name'])
popular_sum = pd.DataFrame(popular_total.count()["Item ID"])
most_pop = popular_sum.sort_values("Item ID", ascending=False)
org_most_pop = most_pop.rename(columns={"Item ID":"Purchase Count"})

total_pop_purc = popular_total["Price"].sum()

#Add Columns
item_total = data_file_df.groupby(["Item Name"])
item_count = pd.DataFrame(item_total.mean())
reduced_item_count = item_count.loc[:, ["Price"]]
id_count = item_count.loc[:, ["Item ID"]]

org_most_pop["Item id"] = item_count["Item ID"]
org_most_pop["Item Price"] = reduced_item_count["Price"]
org_most_pop["Total Purchase Value"] = total_pop_purc

#format
org_most_pop["Total Purchase Value"] = org_most_pop["Total Purchase Value"].map("${:.2f}".format)
org_most_pop["Item Price"] = org_most_pop["Item Price"].map("${:.2f}".format)
org_most_pop["Item id"] = org_most_pop["Item id"].map("{:.0f}".format)

#Display
org_most_pop.head(5)
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
      <th>Item id</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>14</td>
      <td>96</td>
      <td>$2.76</td>
      <td>$38.60</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>84</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>39</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>10</td>
      <td>105</td>
      <td>$3.46</td>
      <td>$34.65</td>
    </tr>
    <tr>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>175</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most profitable
prof_total = data_file_df.groupby(['Item Name'])
prof_sum = pd.DataFrame(prof_total.sum()["Price"])
most_prof = prof_sum.sort_values("Price", ascending=False)

#Rename Columns
org_most_prof = most_prof.rename(columns={"Price":"Total Purchase Value"})

#Add Columns
org_most_prof["Purchase Count"] = prof_total["Item Name"].count()
org_most_prof["Item Price"] = reduced_item_count["Price"]
org_most_prof["Item ID"] = item_count["Item ID"]

#Format
org_most_prof["Total Purchase Value"] = org_most_prof["Total Purchase Value"].map("${:.2f}".format)
org_most_prof["Item Price"] = org_most_prof["Item Price"].map("${:.2f}".format)
org_most_prof["Item ID"] = org_most_prof["Item ID"].map("{:.0f}".format)

#Display
org_most_prof.head(5)
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
      <th>Total Purchase Value</th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Item ID</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>$38.60</td>
      <td>14</td>
      <td>$2.76</td>
      <td>96</td>
    </tr>
    <tr>
      <th>Retribution Axe</th>
      <td>$37.26</td>
      <td>9</td>
      <td>$4.14</td>
      <td>34</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>$34.65</td>
      <td>10</td>
      <td>$3.46</td>
      <td>105</td>
    </tr>
    <tr>
      <th>Spectral Diamond Doomblade</th>
      <td>$29.75</td>
      <td>7</td>
      <td>$4.25</td>
      <td>115</td>
    </tr>
    <tr>
      <th>Orenmir</th>
      <td>$29.70</td>
      <td>6</td>
      <td>$4.95</td>
      <td>32</td>
    </tr>
  </tbody>
</table>
</div>


