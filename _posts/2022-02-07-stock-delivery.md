# Stock Delivery Analysis using Pandas



## Get Stock Delivery Data 
Now let's start working on analysis.
In this blog we will explore how to get delivery data and calculate 10 days and 20 days delivery average of stock
1. Import the necessary library

```python
import pandas as pd
from nsepy import get_history
from datetime import datetime, timedelta, date
import matplotlib.pyplot as plt
```

2. Set start and end date to get data within date range

```python
number_of_days = 50
end_date = datetime.now().date() + timedelta(1)
start_date = end_date - timedelta(number_of_days)
# so here we set date range for last 50 days because we need to get data for last 50 days
```

3. Now get the data into pandas data frame using get_history function which we impored earlier

```python
df = get_history(symbol="tatasteel", start=start_date, end=end_date)
```

so now we have data for last 50 days of tatasteel stock.Same way you can use any stock symbol for which you want to do analysis.


4. Print df.head() which will print first 5 records

```python
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
      <th>Symbol</th>
      <th>Series</th>
      <th>Prev Close</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Last</th>
      <th>Close</th>
      <th>VWAP</th>
      <th>Volume</th>
      <th>Turnover</th>
      <th>Trades</th>
      <th>Deliverable Volume</th>
      <th>%Deliverble</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-12-20</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1132.10</td>
      <td>1117.00</td>
      <td>1117.20</td>
      <td>1066.05</td>
      <td>1073.00</td>
      <td>1072.95</td>
      <td>1082.82</td>
      <td>7352650</td>
      <td>7.961619e+14</td>
      <td>236252</td>
      <td>2370671</td>
      <td>0.3224</td>
    </tr>
    <tr>
      <th>2021-12-21</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1072.95</td>
      <td>1090.00</td>
      <td>1124.25</td>
      <td>1085.00</td>
      <td>1108.85</td>
      <td>1105.10</td>
      <td>1109.16</td>
      <td>7026849</td>
      <td>7.793906e+14</td>
      <td>171193</td>
      <td>1594912</td>
      <td>0.2270</td>
    </tr>
    <tr>
      <th>2021-12-22</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1105.10</td>
      <td>1115.00</td>
      <td>1131.00</td>
      <td>1110.00</td>
      <td>1130.00</td>
      <td>1128.85</td>
      <td>1123.02</td>
      <td>4366917</td>
      <td>4.904149e+14</td>
      <td>114115</td>
      <td>1005289</td>
      <td>0.2302</td>
    </tr>
    <tr>
      <th>2021-12-23</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1128.85</td>
      <td>1138.00</td>
      <td>1146.80</td>
      <td>1120.20</td>
      <td>1126.45</td>
      <td>1127.65</td>
      <td>1134.88</td>
      <td>4621478</td>
      <td>5.244827e+14</td>
      <td>114823</td>
      <td>1226564</td>
      <td>0.2654</td>
    </tr>
    <tr>
      <th>2021-12-24</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1127.65</td>
      <td>1130.05</td>
      <td>1133.45</td>
      <td>1103.85</td>
      <td>1117.00</td>
      <td>1115.45</td>
      <td>1115.11</td>
      <td>4093630</td>
      <td>4.564829e+14</td>
      <td>96182</td>
      <td>824805</td>
      <td>0.2015</td>
    </tr>
  </tbody>
</table>
</div>



In above table get_history fundtion return so many columns. For the scope of this blog we will focus on following column only

    1. date
    2. Symbol
    3. Series
    4. Volume
    5. Deliverable Volume --> This is exactly we need for our analysis

Let's create two new columns in dataframe which will hold delivery volume average of 10 days and 20 days each

```python
df['delivery_SMA_10'] = df.iloc[:,12].rolling(window=10).mean()
df['delivery_SMA_20'] = df.iloc[:,12].rolling(window=20).mean()
```

In above code ```df.iloc[:,12]``` will return all values from 'Deliverable Volume' column and also we used rolling function to find out mean of underlying data having a window size of 10 and 20.

Now Print first 30 rows

```python
df.head(30)
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
      <th>Symbol</th>
      <th>Series</th>
      <th>Prev Close</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Last</th>
      <th>Close</th>
      <th>VWAP</th>
      <th>Volume</th>
      <th>Turnover</th>
      <th>Trades</th>
      <th>Deliverable Volume</th>
      <th>%Deliverble</th>
      <th>delivery_SMA_10</th>
      <th>delivery_SMA_20</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-12-20</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1132.10</td>
      <td>1117.00</td>
      <td>1117.20</td>
      <td>1066.05</td>
      <td>1073.00</td>
      <td>1072.95</td>
      <td>1082.82</td>
      <td>7352650</td>
      <td>7.961619e+14</td>
      <td>236252</td>
      <td>2370671</td>
      <td>0.3224</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2021-12-21</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1072.95</td>
      <td>1090.00</td>
      <td>1124.25</td>
      <td>1085.00</td>
      <td>1108.85</td>
      <td>1105.10</td>
      <td>1109.16</td>
      <td>7026849</td>
      <td>7.793906e+14</td>
      <td>171193</td>
      <td>1594912</td>
      <td>0.2270</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2021-12-22</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1105.10</td>
      <td>1115.00</td>
      <td>1131.00</td>
      <td>1110.00</td>
      <td>1130.00</td>
      <td>1128.85</td>
      <td>1123.02</td>
      <td>4366917</td>
      <td>4.904149e+14</td>
      <td>114115</td>
      <td>1005289</td>
      <td>0.2302</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2021-12-23</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1128.85</td>
      <td>1138.00</td>
      <td>1146.80</td>
      <td>1120.20</td>
      <td>1126.45</td>
      <td>1127.65</td>
      <td>1134.88</td>
      <td>4621478</td>
      <td>5.244827e+14</td>
      <td>114823</td>
      <td>1226564</td>
      <td>0.2654</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2021-12-24</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1127.65</td>
      <td>1130.05</td>
      <td>1133.45</td>
      <td>1103.85</td>
      <td>1117.00</td>
      <td>1115.45</td>
      <td>1115.11</td>
      <td>4093630</td>
      <td>4.564829e+14</td>
      <td>96182</td>
      <td>824805</td>
      <td>0.2015</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2021-12-27</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1115.45</td>
      <td>1111.05</td>
      <td>1124.70</td>
      <td>1102.35</td>
      <td>1124.00</td>
      <td>1121.80</td>
      <td>1114.49</td>
      <td>2758851</td>
      <td>3.074721e+14</td>
      <td>74732</td>
      <td>368587</td>
      <td>0.1336</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2021-12-28</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1121.80</td>
      <td>1127.00</td>
      <td>1131.50</td>
      <td>1122.00</td>
      <td>1127.00</td>
      <td>1127.45</td>
      <td>1127.24</td>
      <td>2880781</td>
      <td>3.247338e+14</td>
      <td>108720</td>
      <td>1134146</td>
      <td>0.3937</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2021-12-29</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1127.45</td>
      <td>1120.00</td>
      <td>1126.65</td>
      <td>1108.00</td>
      <td>1115.75</td>
      <td>1116.25</td>
      <td>1115.35</td>
      <td>4193520</td>
      <td>4.677260e+14</td>
      <td>145576</td>
      <td>1651673</td>
      <td>0.3939</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2021-12-30</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1116.25</td>
      <td>1117.90</td>
      <td>1127.00</td>
      <td>1099.00</td>
      <td>1101.15</td>
      <td>1101.00</td>
      <td>1110.09</td>
      <td>4114093</td>
      <td>4.567033e+14</td>
      <td>102093</td>
      <td>1509421</td>
      <td>0.3669</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2021-12-31</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1101.00</td>
      <td>1105.00</td>
      <td>1123.50</td>
      <td>1102.65</td>
      <td>1112.40</td>
      <td>1111.45</td>
      <td>1115.49</td>
      <td>3687021</td>
      <td>4.112829e+14</td>
      <td>94613</td>
      <td>1171618</td>
      <td>0.3178</td>
      <td>1285768.6</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2022-01-03</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1111.45</td>
      <td>1115.00</td>
      <td>1151.00</td>
      <td>1115.00</td>
      <td>1150.00</td>
      <td>1142.45</td>
      <td>1129.46</td>
      <td>3865803</td>
      <td>4.366280e+14</td>
      <td>87982</td>
      <td>1215520</td>
      <td>0.3144</td>
      <td>1170253.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2022-01-04</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1142.45</td>
      <td>1153.00</td>
      <td>1159.70</td>
      <td>1136.50</td>
      <td>1148.05</td>
      <td>1148.80</td>
      <td>1147.81</td>
      <td>5975731</td>
      <td>6.859027e+14</td>
      <td>125400</td>
      <td>1733910</td>
      <td>0.2902</td>
      <td>1184153.3</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2022-01-05</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1148.80</td>
      <td>1147.00</td>
      <td>1180.80</td>
      <td>1141.25</td>
      <td>1176.00</td>
      <td>1177.60</td>
      <td>1166.60</td>
      <td>6186176</td>
      <td>7.216800e+14</td>
      <td>134032</td>
      <td>1997541</td>
      <td>0.3229</td>
      <td>1283378.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2022-01-06</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1177.60</td>
      <td>1172.00</td>
      <td>1183.00</td>
      <td>1155.55</td>
      <td>1161.50</td>
      <td>1163.25</td>
      <td>1166.83</td>
      <td>5335400</td>
      <td>6.225489e+14</td>
      <td>133187</td>
      <td>1440528</td>
      <td>0.2700</td>
      <td>1304774.9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2022-01-07</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1163.25</td>
      <td>1165.20</td>
      <td>1174.00</td>
      <td>1147.85</td>
      <td>1158.15</td>
      <td>1160.35</td>
      <td>1160.62</td>
      <td>3973857</td>
      <td>4.612154e+14</td>
      <td>87679</td>
      <td>1167664</td>
      <td>0.2938</td>
      <td>1339060.8</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2022-01-10</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1160.35</td>
      <td>1165.00</td>
      <td>1173.40</td>
      <td>1153.85</td>
      <td>1168.00</td>
      <td>1169.05</td>
      <td>1165.95</td>
      <td>3822500</td>
      <td>4.456832e+14</td>
      <td>124004</td>
      <td>1331943</td>
      <td>0.3484</td>
      <td>1435396.4</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2022-01-11</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1169.05</td>
      <td>1158.65</td>
      <td>1162.70</td>
      <td>1113.00</td>
      <td>1134.90</td>
      <td>1130.25</td>
      <td>1137.02</td>
      <td>9697885</td>
      <td>1.102671e+15</td>
      <td>245197</td>
      <td>3285797</td>
      <td>0.3388</td>
      <td>1650561.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2022-01-12</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1130.25</td>
      <td>1139.85</td>
      <td>1150.80</td>
      <td>1132.40</td>
      <td>1147.80</td>
      <td>1147.20</td>
      <td>1142.13</td>
      <td>5035843</td>
      <td>5.751572e+14</td>
      <td>139132</td>
      <td>1525010</td>
      <td>0.3028</td>
      <td>1637895.2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2022-01-13</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1147.20</td>
      <td>1154.00</td>
      <td>1225.50</td>
      <td>1152.15</td>
      <td>1219.00</td>
      <td>1221.15</td>
      <td>1200.08</td>
      <td>15723116</td>
      <td>1.886896e+15</td>
      <td>350847</td>
      <td>4326987</td>
      <td>0.2752</td>
      <td>1919651.8</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2022-01-14</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1221.15</td>
      <td>1213.00</td>
      <td>1226.30</td>
      <td>1206.10</td>
      <td>1211.00</td>
      <td>1213.60</td>
      <td>1215.42</td>
      <td>5267907</td>
      <td>6.402741e+14</td>
      <td>162565</td>
      <td>1290762</td>
      <td>0.2450</td>
      <td>1931566.2</td>
      <td>1608667.40</td>
    </tr>
    <tr>
      <th>2022-01-17</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1213.60</td>
      <td>1215.00</td>
      <td>1245.00</td>
      <td>1190.35</td>
      <td>1230.80</td>
      <td>1229.75</td>
      <td>1221.77</td>
      <td>8969607</td>
      <td>1.095879e+15</td>
      <td>202840</td>
      <td>2210957</td>
      <td>0.2465</td>
      <td>2031109.9</td>
      <td>1600681.70</td>
    </tr>
    <tr>
      <th>2022-01-18</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1229.75</td>
      <td>1230.00</td>
      <td>1233.75</td>
      <td>1190.00</td>
      <td>1196.95</td>
      <td>1194.85</td>
      <td>1209.80</td>
      <td>5840897</td>
      <td>7.066337e+14</td>
      <td>150353</td>
      <td>1797537</td>
      <td>0.3078</td>
      <td>2037472.6</td>
      <td>1610812.95</td>
    </tr>
    <tr>
      <th>2022-01-19</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1194.85</td>
      <td>1196.00</td>
      <td>1220.90</td>
      <td>1183.00</td>
      <td>1208.00</td>
      <td>1209.60</td>
      <td>1204.89</td>
      <td>7304321</td>
      <td>8.800884e+14</td>
      <td>186907</td>
      <td>1335466</td>
      <td>0.1828</td>
      <td>1971265.1</td>
      <td>1627321.80</td>
    </tr>
    <tr>
      <th>2022-01-20</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1209.60</td>
      <td>1210.10</td>
      <td>1224.00</td>
      <td>1197.00</td>
      <td>1209.90</td>
      <td>1206.70</td>
      <td>1212.03</td>
      <td>5920597</td>
      <td>7.175913e+14</td>
      <td>159207</td>
      <td>1526225</td>
      <td>0.2578</td>
      <td>1979834.8</td>
      <td>1642304.85</td>
    </tr>
    <tr>
      <th>2022-01-21</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1206.70</td>
      <td>1200.00</td>
      <td>1204.30</td>
      <td>1156.35</td>
      <td>1169.10</td>
      <td>1169.70</td>
      <td>1177.49</td>
      <td>7166267</td>
      <td>8.438184e+14</td>
      <td>170921</td>
      <td>2048736</td>
      <td>0.2859</td>
      <td>2067942.0</td>
      <td>1703501.40</td>
    </tr>
    <tr>
      <th>2022-01-24</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1169.70</td>
      <td>1160.00</td>
      <td>1163.65</td>
      <td>1090.00</td>
      <td>1100.50</td>
      <td>1099.20</td>
      <td>1121.02</td>
      <td>8392076</td>
      <td>9.407700e+14</td>
      <td>279320</td>
      <td>1468792</td>
      <td>0.2910</td>
      <td>2081626.9</td>
      <td>1758511.65</td>
    </tr>
    <tr>
      <th>2022-01-25</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1099.20</td>
      <td>1094.70</td>
      <td>1121.00</td>
      <td>1087.05</td>
      <td>1105.95</td>
      <td>1109.10</td>
      <td>1107.84</td>
      <td>5733063</td>
      <td>6.351335e+14</td>
      <td>206703</td>
      <td>1555235</td>
      <td>0.2713</td>
      <td>1908570.7</td>
      <td>1779566.10</td>
    </tr>
    <tr>
      <th>2022-01-27</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1109.10</td>
      <td>1090.00</td>
      <td>1103.45</td>
      <td>1061.30</td>
      <td>1090.65</td>
      <td>1088.35</td>
      <td>1082.03</td>
      <td>9056656</td>
      <td>9.799578e+14</td>
      <td>271554</td>
      <td>2244835</td>
      <td>0.2479</td>
      <td>1980553.2</td>
      <td>1809224.20</td>
    </tr>
    <tr>
      <th>2022-01-28</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1088.35</td>
      <td>1105.85</td>
      <td>1125.75</td>
      <td>1080.95</td>
      <td>1086.00</td>
      <td>1084.65</td>
      <td>1108.99</td>
      <td>7864893</td>
      <td>8.722068e+14</td>
      <td>192376</td>
      <td>2044249</td>
      <td>0.2599</td>
      <td>1752279.4</td>
      <td>1835965.60</td>
    </tr>
    <tr>
      <th>2022-01-31</th>
      <td>TATASTEEL</td>
      <td>EQ</td>
      <td>1084.65</td>
      <td>1109.90</td>
      <td>1114.60</td>
      <td>1080.50</td>
      <td>1085.00</td>
      <td>1085.55</td>
      <td>1096.19</td>
      <td>5570939</td>
      <td>6.106809e+14</td>
      <td>172294</td>
      <td>1283038</td>
      <td>0.2303</td>
      <td>1751507.0</td>
      <td>1841536.60</td>
    </tr>
  </tbody>
</table>
</div>



In above table two additional column added i.e 'delivery_SMA_10' and 'delivery_SMA_20' which hold average volume of previous 10 days and 20 days respectivly. First 10 records for delivery_SMA_10 and 20 records for delivery_SMA_20 is NaN because for those previous 10 days and 20 days volume data are not available. 

Now we can plot graph of volume average to make sense of above data as "picture tells a thousand words"

```python
# Remove all Nan Value 
df.dropna(inplace=True)
# Plot 10 days average volume line with green color
plt.plot(df.index, df['delivery_SMA_10'], label='SMA 10 days delivery', color='g')

# # Plot 20 days average volume line with red color
plt.plot(df.index, df['delivery_SMA_20'], label='SMA 20 days delivery', color='r')

plt.legend()
plt.show() 
```


![png](/images/stock_delivery_files/output_15_0.png)


In above graph we plot lines of 10 days moving average of Deliverable Volume and 20 days moving average Deliverable Volume.

Above graph is showing us that as of current date(2022-02-05) 10 days moving average is less than 20 days moving average so we can clerly undestand Delivery start decreasing when 10 days delivery line cross 20 days delivery line from above.

Delivery analysis is one of the way to analyse stock data which you can include in your trading strategy. 

In this article I write about how we can use stock delivery data to calculate average and plot graph based on it.
We can use this data in many ways to derive what is happening with the stock in trading session. 

Here we calculate 10 days and 20 days movine average of volume. But you can calculate the same for any number of days.

In this article I discussed about only Deliverable Volume data but you can do analysis on other data from table like Open, Close, High, Low, VWAP, Turnover and also plot graph based on your calculated data. 
I will soon write on different kind of analysis which we can do from stock history data using Pandas.
