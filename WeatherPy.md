

```python
from citipy import citipy
import csv
import kdtree
import os
import random
import pandas as pd
import numpy as np
import requests
import json
import matplotlib.pyplot as plt
import openweathermapy as ow

#test that citpy library is working
city = citipy.nearest_city(22.99, 120.21)
city

print(city.city_name)     # Tainan, my home town

print(city.country_code)   # And the country is surely Taiwan
```

    tainan
    tw



```python
#use numpy random unifrom function to get evently spaced latitudes and longitudes (-90 to 90, -180 to 180)
#populate one list for lat, one for long
lat_list = np.random.uniform(-90, 90, 550)
lon_list = np.random.uniform(-180, 180, 550)

#print(lat_list)
#print(lon_list)

```


```python
#use citipy to generate cities and country code for those lists
city_list = []
country_list = []

for (lat, lon) in zip(lat_list, lon_list):
    
    city = citipy.nearest_city(lat, lon)
  
    city_list.append(city.city_name)
    country_list.append(city.country_code)
    
#print(city_list)
#print(country_list)
```


```python
#turn that into a dataframe
cities_df = pd.DataFrame(np.column_stack([lat_list, lon_list, city_list, country_list]), 
                               columns=['Input Latitude', 'Input Longitude', 'City', 'Country Code'])
cities_df.head()
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
      <th>Input Latitude</th>
      <th>Input Longitude</th>
      <th>City</th>
      <th>Country Code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>40.19816761212215</td>
      <td>80.62127019345218</td>
      <td>aksu</td>
      <td>cn</td>
    </tr>
    <tr>
      <th>1</th>
      <td>82.7617171808077</td>
      <td>67.67656031389171</td>
      <td>amderma</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>2</th>
      <td>24.385027496903774</td>
      <td>65.8441042214397</td>
      <td>karachi</td>
      <td>pk</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-62.83620024644605</td>
      <td>-94.66579007448047</td>
      <td>punta arenas</td>
      <td>cl</td>
    </tr>
    <tr>
      <th>4</th>
      <td>82.60974539014194</td>
      <td>90.69761643141709</td>
      <td>talnakh</td>
      <td>ru</td>
    </tr>
  </tbody>
</table>
</div>




```python
#take the inputs out of df so we have a clean df to send to csv
reduced_cities_df = cities_df[['City', 'Country Code']]
print(reduced_cities_df)
```

                           City Country Code
    0                      aksu           cn
    1                   amderma           ru
    2                   karachi           pk
    3              punta arenas           cl
    4                   talnakh           ru
    5                    malwan           in
    6               new norfolk           au
    7                   mataura           pf
    8                 ilulissat           gl
    9                  klaksvik           fo
    10                   playas           ec
    11                  rikitea           pf
    12                 prenzlau           de
    13              new norfolk           au
    14                   dikson           ru
    15              nizhneyansk           ru
    16                    bluff           nz
    17                   halalo           wf
    18               bredasdorp           za
    19                taolanaro           mg
    20                     esna           eg
    21                  ushuaia           ar
    22                    kapaa           us
    23                  ushuaia           ar
    24                 veracruz           mx
    25                    truro           gb
    26                  rikitea           pf
    27                antsohihy           mg
    28             saint-joseph           re
    29                   bethel           us
    ..                      ...          ...
    520                 kismayo           so
    521            saint george           bm
    522                  bethel           us
    523          saint-philippe           re
    524                 okhotsk           ru
    525                   genhe           cn
    526                  barrow           us
    527            saint george           bm
    528               cape town           za
    529             barentsburg           sj
    530                 paravur           in
    531               piacabucu           br
    532                kamaishi           jp
    533             port alfred           za
    534           half moon bay           us
    535                  cosala           mx
    536                belmonte           br
    537            punta arenas           cl
    538                  agadir           ma
    539  grand river south east           mu
    540             yellowknife           ca
    541                  albany           au
    542                  rawson           ar
    543                 rikitea           pf
    544                policoro           it
    545                khatanga           ru
    546                katsuura           jp
    547                 rikitea           pf
    548                   ulkan           ru
    549               tazovskiy           ru
    
    [550 rows x 2 columns]



```python
#turn it into a csv inorder to hit the openweather api, loop through 
reduced_cities_df.to_csv("Output/cities.csv", encoding="utf-8", index=False, header=False)
```


```python
# Create a settings object with your API key and preferred units
#api_key = "85e7df885565606034ad67f99611b82f"

#settings = {"units": "imperial", "appid": api_key}

#cities = []
#real_lat = []
#real_lon = []
#temp = []
#hum = []
#cloud = []
#wind = []
```


```python
# Get data for each city in cities.csv
#with open("output/cities.csv") as cityfile:
    #cityData = csv.reader(cityfile)
    #for city in cityData:
        #cities.append(city[0])

#print(cities)    
    
#for city in cities:
    #weather_data = ow.get_current(city, **settings)
    #real_lat.append(weather_data["coord"]["lat"])
    #real_lon.append(weather_data["coord"]["lon"])
    #temp.append(weather_data["main"]["temp"])
    #hum.append(weather_data["main"]["humidity"])
    #cloud.append(weather_data["clouds"]["all"])
    #wind.append(weather_data["wind"]["speed"])

#print(real_lat)
#print(real_lon)
#print(temp)
#print(hum)
#print(cloud)
#print(wind)


```


```python
#api.openweathermap.org/data/2.5/weather?q={city name},{country code}
# Save config information.
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "imperial"
api_key = "85e7df885565606034ad67f99611b82f"


# Build partial query URL
query_url = f"{url}appid={api_key}&units={units}&q="


```


```python
# Pretty print the json for a test country to see the response data
#response = requests.get(query_url + "saint george" + "," + "bm")
#print(json.dumps(response, indent=4, sort_keys=True))
```


```python
# set up lists to hold reponse info
real_lat = []
real_lon = []
temp = []
hum = []
cloud = []
wind = []

# Loop through the list of cities and perform a request for data on each
# Sometimes getting an error for the coord of a city, so incorporate a try-except to skip any that 
#are missing a data point.


for (city, country) in zip(city_list, country_list):
    try:
        city_data = requests.get(query_url + city).json()
        real_lat.append(city_data["coord"]["lat"])
        real_lon.append(city_data["coord"]["lon"])
        temp.append(city_data["main"]["temp"])
        hum.append(city_data["main"]["humidity"])
        cloud.append(city_data["clouds"]["all"])
        wind.append(city_data["wind"]["speed"])
    except (KeyError, ValueError):
        real_lat.append(None)
        real_lon.append(None)
        temp.append(None)
        hum.append(None)
        cloud.append(None)
        wind.append(None)
    
#print(f"The latitude information received is: {real_lat}")
#print(f"The longitude information received is: {real_lon}")
#print(f"The temperature information received is: {temp}")
#print(f"The humidity information received is: {hum}")
#print(f"The cloudiness information received is: {cloud}")
#print(f"The wind speed information received is: {wind}")



```


```python
weather_dict = {
    "city": city_list,
    "country code": country_list,
    "lat": real_lat,
    "lon": real_lon,
    "temp": temp,
    "humidity": hum,
    "cloudiness": cloud,
    "wind speed": wind
}
weather_data = pd.DataFrame(weather_dict)
weather_data.head(15)
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
      <th>city</th>
      <th>cloudiness</th>
      <th>country code</th>
      <th>humidity</th>
      <th>lat</th>
      <th>lon</th>
      <th>temp</th>
      <th>wind speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>aksu</td>
      <td>75.0</td>
      <td>cn</td>
      <td>87.0</td>
      <td>52.04</td>
      <td>76.93</td>
      <td>32.00</td>
      <td>13.42</td>
    </tr>
    <tr>
      <th>1</th>
      <td>amderma</td>
      <td>NaN</td>
      <td>ru</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>karachi</td>
      <td>0.0</td>
      <td>pk</td>
      <td>69.0</td>
      <td>24.87</td>
      <td>67.03</td>
      <td>80.60</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>3</th>
      <td>punta arenas</td>
      <td>0.0</td>
      <td>cl</td>
      <td>86.0</td>
      <td>-53.16</td>
      <td>-70.91</td>
      <td>39.20</td>
      <td>14.67</td>
    </tr>
    <tr>
      <th>4</th>
      <td>talnakh</td>
      <td>12.0</td>
      <td>ru</td>
      <td>91.0</td>
      <td>69.49</td>
      <td>88.39</td>
      <td>-7.46</td>
      <td>4.16</td>
    </tr>
    <tr>
      <th>5</th>
      <td>malwan</td>
      <td>NaN</td>
      <td>in</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>new norfolk</td>
      <td>20.0</td>
      <td>au</td>
      <td>40.0</td>
      <td>-42.78</td>
      <td>147.06</td>
      <td>71.60</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>7</th>
      <td>mataura</td>
      <td>92.0</td>
      <td>pf</td>
      <td>73.0</td>
      <td>-46.19</td>
      <td>168.86</td>
      <td>59.15</td>
      <td>27.98</td>
    </tr>
    <tr>
      <th>8</th>
      <td>ilulissat</td>
      <td>20.0</td>
      <td>gl</td>
      <td>78.0</td>
      <td>69.22</td>
      <td>-51.10</td>
      <td>10.40</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>9</th>
      <td>klaksvik</td>
      <td>48.0</td>
      <td>fo</td>
      <td>86.0</td>
      <td>62.23</td>
      <td>-6.59</td>
      <td>26.60</td>
      <td>14.99</td>
    </tr>
    <tr>
      <th>10</th>
      <td>playas</td>
      <td>76.0</td>
      <td>ec</td>
      <td>95.0</td>
      <td>-2.64</td>
      <td>-80.39</td>
      <td>73.82</td>
      <td>4.38</td>
    </tr>
    <tr>
      <th>11</th>
      <td>rikitea</td>
      <td>20.0</td>
      <td>pf</td>
      <td>100.0</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>79.04</td>
      <td>4.94</td>
    </tr>
    <tr>
      <th>12</th>
      <td>prenzlau</td>
      <td>92.0</td>
      <td>de</td>
      <td>93.0</td>
      <td>53.32</td>
      <td>13.87</td>
      <td>35.21</td>
      <td>11.43</td>
    </tr>
    <tr>
      <th>13</th>
      <td>new norfolk</td>
      <td>20.0</td>
      <td>au</td>
      <td>40.0</td>
      <td>-42.78</td>
      <td>147.06</td>
      <td>71.60</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>14</th>
      <td>dikson</td>
      <td>36.0</td>
      <td>ru</td>
      <td>91.0</td>
      <td>73.51</td>
      <td>80.55</td>
      <td>1.55</td>
      <td>18.81</td>
    </tr>
  </tbody>
</table>
</div>




```python
#drop NaNs
weather_data.dropna()
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
      <th>city</th>
      <th>cloudiness</th>
      <th>country code</th>
      <th>humidity</th>
      <th>lat</th>
      <th>lon</th>
      <th>temp</th>
      <th>wind speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>aksu</td>
      <td>75.0</td>
      <td>cn</td>
      <td>87.0</td>
      <td>52.04</td>
      <td>76.93</td>
      <td>32.00</td>
      <td>13.42</td>
    </tr>
    <tr>
      <th>2</th>
      <td>karachi</td>
      <td>0.0</td>
      <td>pk</td>
      <td>69.0</td>
      <td>24.87</td>
      <td>67.03</td>
      <td>80.60</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>3</th>
      <td>punta arenas</td>
      <td>0.0</td>
      <td>cl</td>
      <td>86.0</td>
      <td>-53.16</td>
      <td>-70.91</td>
      <td>39.20</td>
      <td>14.67</td>
    </tr>
    <tr>
      <th>4</th>
      <td>talnakh</td>
      <td>12.0</td>
      <td>ru</td>
      <td>91.0</td>
      <td>69.49</td>
      <td>88.39</td>
      <td>-7.46</td>
      <td>4.16</td>
    </tr>
    <tr>
      <th>6</th>
      <td>new norfolk</td>
      <td>20.0</td>
      <td>au</td>
      <td>40.0</td>
      <td>-42.78</td>
      <td>147.06</td>
      <td>71.60</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>7</th>
      <td>mataura</td>
      <td>92.0</td>
      <td>pf</td>
      <td>73.0</td>
      <td>-46.19</td>
      <td>168.86</td>
      <td>59.15</td>
      <td>27.98</td>
    </tr>
    <tr>
      <th>8</th>
      <td>ilulissat</td>
      <td>20.0</td>
      <td>gl</td>
      <td>78.0</td>
      <td>69.22</td>
      <td>-51.10</td>
      <td>10.40</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>9</th>
      <td>klaksvik</td>
      <td>48.0</td>
      <td>fo</td>
      <td>86.0</td>
      <td>62.23</td>
      <td>-6.59</td>
      <td>26.60</td>
      <td>14.99</td>
    </tr>
    <tr>
      <th>10</th>
      <td>playas</td>
      <td>76.0</td>
      <td>ec</td>
      <td>95.0</td>
      <td>-2.64</td>
      <td>-80.39</td>
      <td>73.82</td>
      <td>4.38</td>
    </tr>
    <tr>
      <th>11</th>
      <td>rikitea</td>
      <td>20.0</td>
      <td>pf</td>
      <td>100.0</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>79.04</td>
      <td>4.94</td>
    </tr>
    <tr>
      <th>12</th>
      <td>prenzlau</td>
      <td>92.0</td>
      <td>de</td>
      <td>93.0</td>
      <td>53.32</td>
      <td>13.87</td>
      <td>35.21</td>
      <td>11.43</td>
    </tr>
    <tr>
      <th>13</th>
      <td>new norfolk</td>
      <td>20.0</td>
      <td>au</td>
      <td>40.0</td>
      <td>-42.78</td>
      <td>147.06</td>
      <td>71.60</td>
      <td>17.22</td>
    </tr>
    <tr>
      <th>14</th>
      <td>dikson</td>
      <td>36.0</td>
      <td>ru</td>
      <td>91.0</td>
      <td>73.51</td>
      <td>80.55</td>
      <td>1.55</td>
      <td>18.81</td>
    </tr>
    <tr>
      <th>16</th>
      <td>bluff</td>
      <td>88.0</td>
      <td>nz</td>
      <td>46.0</td>
      <td>-23.58</td>
      <td>149.07</td>
      <td>84.44</td>
      <td>16.02</td>
    </tr>
    <tr>
      <th>18</th>
      <td>bredasdorp</td>
      <td>8.0</td>
      <td>za</td>
      <td>87.0</td>
      <td>-34.53</td>
      <td>20.04</td>
      <td>55.40</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>20</th>
      <td>esna</td>
      <td>0.0</td>
      <td>eg</td>
      <td>83.0</td>
      <td>45.20</td>
      <td>27.57</td>
      <td>46.55</td>
      <td>5.95</td>
    </tr>
    <tr>
      <th>21</th>
      <td>ushuaia</td>
      <td>75.0</td>
      <td>ar</td>
      <td>80.0</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>41.00</td>
      <td>14.99</td>
    </tr>
    <tr>
      <th>22</th>
      <td>kapaa</td>
      <td>90.0</td>
      <td>us</td>
      <td>69.0</td>
      <td>22.08</td>
      <td>-159.32</td>
      <td>74.07</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>23</th>
      <td>ushuaia</td>
      <td>75.0</td>
      <td>ar</td>
      <td>80.0</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>41.00</td>
      <td>14.99</td>
    </tr>
    <tr>
      <th>24</th>
      <td>veracruz</td>
      <td>75.0</td>
      <td>mx</td>
      <td>100.0</td>
      <td>7.66</td>
      <td>-72.25</td>
      <td>75.20</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>25</th>
      <td>truro</td>
      <td>75.0</td>
      <td>gb</td>
      <td>93.0</td>
      <td>50.26</td>
      <td>-5.05</td>
      <td>50.00</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>26</th>
      <td>rikitea</td>
      <td>20.0</td>
      <td>pf</td>
      <td>100.0</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>79.04</td>
      <td>4.94</td>
    </tr>
    <tr>
      <th>27</th>
      <td>antsohihy</td>
      <td>0.0</td>
      <td>mg</td>
      <td>93.0</td>
      <td>-14.88</td>
      <td>47.99</td>
      <td>82.01</td>
      <td>3.83</td>
    </tr>
    <tr>
      <th>28</th>
      <td>saint-joseph</td>
      <td>92.0</td>
      <td>re</td>
      <td>66.0</td>
      <td>43.56</td>
      <td>6.97</td>
      <td>55.40</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>29</th>
      <td>bethel</td>
      <td>1.0</td>
      <td>us</td>
      <td>79.0</td>
      <td>60.79</td>
      <td>-161.76</td>
      <td>32.00</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>30</th>
      <td>bodden town</td>
      <td>56.0</td>
      <td>ky</td>
      <td>100.0</td>
      <td>19.28</td>
      <td>-81.25</td>
      <td>78.59</td>
      <td>10.98</td>
    </tr>
    <tr>
      <th>31</th>
      <td>dunedin</td>
      <td>12.0</td>
      <td>nz</td>
      <td>43.0</td>
      <td>-45.87</td>
      <td>170.50</td>
      <td>62.66</td>
      <td>12.55</td>
    </tr>
    <tr>
      <th>32</th>
      <td>kimbe</td>
      <td>88.0</td>
      <td>pg</td>
      <td>100.0</td>
      <td>-5.56</td>
      <td>150.15</td>
      <td>78.05</td>
      <td>4.50</td>
    </tr>
    <tr>
      <th>33</th>
      <td>codrington</td>
      <td>92.0</td>
      <td>ag</td>
      <td>89.0</td>
      <td>-28.95</td>
      <td>153.24</td>
      <td>71.30</td>
      <td>4.05</td>
    </tr>
    <tr>
      <th>36</th>
      <td>ushuaia</td>
      <td>75.0</td>
      <td>ar</td>
      <td>80.0</td>
      <td>-54.81</td>
      <td>-68.31</td>
      <td>41.00</td>
      <td>14.99</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>516</th>
      <td>mayo</td>
      <td>40.0</td>
      <td>ca</td>
      <td>36.0</td>
      <td>63.59</td>
      <td>-135.90</td>
      <td>14.00</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>517</th>
      <td>vostok</td>
      <td>76.0</td>
      <td>ru</td>
      <td>53.0</td>
      <td>46.45</td>
      <td>135.83</td>
      <td>30.89</td>
      <td>8.63</td>
    </tr>
    <tr>
      <th>518</th>
      <td>rikitea</td>
      <td>20.0</td>
      <td>pf</td>
      <td>100.0</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>79.04</td>
      <td>4.94</td>
    </tr>
    <tr>
      <th>519</th>
      <td>kapaa</td>
      <td>90.0</td>
      <td>us</td>
      <td>69.0</td>
      <td>22.08</td>
      <td>-159.32</td>
      <td>74.07</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>521</th>
      <td>saint george</td>
      <td>20.0</td>
      <td>bm</td>
      <td>87.0</td>
      <td>39.45</td>
      <td>22.34</td>
      <td>42.80</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>522</th>
      <td>bethel</td>
      <td>1.0</td>
      <td>us</td>
      <td>79.0</td>
      <td>60.79</td>
      <td>-161.76</td>
      <td>32.00</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>523</th>
      <td>saint-philippe</td>
      <td>75.0</td>
      <td>re</td>
      <td>75.0</td>
      <td>45.36</td>
      <td>-73.48</td>
      <td>31.51</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>524</th>
      <td>okhotsk</td>
      <td>68.0</td>
      <td>ru</td>
      <td>71.0</td>
      <td>59.36</td>
      <td>143.24</td>
      <td>28.46</td>
      <td>17.69</td>
    </tr>
    <tr>
      <th>525</th>
      <td>genhe</td>
      <td>56.0</td>
      <td>cn</td>
      <td>33.0</td>
      <td>50.78</td>
      <td>121.52</td>
      <td>18.92</td>
      <td>16.02</td>
    </tr>
    <tr>
      <th>526</th>
      <td>barrow</td>
      <td>0.0</td>
      <td>us</td>
      <td>91.0</td>
      <td>-38.31</td>
      <td>-60.23</td>
      <td>54.56</td>
      <td>9.19</td>
    </tr>
    <tr>
      <th>527</th>
      <td>saint george</td>
      <td>20.0</td>
      <td>bm</td>
      <td>87.0</td>
      <td>39.45</td>
      <td>22.34</td>
      <td>42.80</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>528</th>
      <td>cape town</td>
      <td>0.0</td>
      <td>za</td>
      <td>77.0</td>
      <td>-33.93</td>
      <td>18.42</td>
      <td>64.40</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>530</th>
      <td>paravur</td>
      <td>40.0</td>
      <td>in</td>
      <td>66.0</td>
      <td>10.15</td>
      <td>76.23</td>
      <td>87.80</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>531</th>
      <td>piacabucu</td>
      <td>88.0</td>
      <td>br</td>
      <td>100.0</td>
      <td>-10.41</td>
      <td>-36.43</td>
      <td>72.29</td>
      <td>4.05</td>
    </tr>
    <tr>
      <th>532</th>
      <td>kamaishi</td>
      <td>75.0</td>
      <td>jp</td>
      <td>67.0</td>
      <td>39.28</td>
      <td>141.86</td>
      <td>59.00</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>533</th>
      <td>port alfred</td>
      <td>80.0</td>
      <td>za</td>
      <td>95.0</td>
      <td>-33.59</td>
      <td>26.89</td>
      <td>70.85</td>
      <td>10.87</td>
    </tr>
    <tr>
      <th>534</th>
      <td>half moon bay</td>
      <td>1.0</td>
      <td>us</td>
      <td>66.0</td>
      <td>37.46</td>
      <td>-122.43</td>
      <td>55.02</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>536</th>
      <td>belmonte</td>
      <td>92.0</td>
      <td>br</td>
      <td>98.0</td>
      <td>40.36</td>
      <td>-7.35</td>
      <td>48.62</td>
      <td>12.88</td>
    </tr>
    <tr>
      <th>537</th>
      <td>punta arenas</td>
      <td>0.0</td>
      <td>cl</td>
      <td>86.0</td>
      <td>-53.16</td>
      <td>-70.91</td>
      <td>39.20</td>
      <td>14.67</td>
    </tr>
    <tr>
      <th>538</th>
      <td>agadir</td>
      <td>40.0</td>
      <td>ma</td>
      <td>87.0</td>
      <td>30.42</td>
      <td>-9.58</td>
      <td>53.60</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>540</th>
      <td>yellowknife</td>
      <td>20.0</td>
      <td>ca</td>
      <td>58.0</td>
      <td>62.45</td>
      <td>-114.38</td>
      <td>-4.01</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>541</th>
      <td>albany</td>
      <td>75.0</td>
      <td>au</td>
      <td>64.0</td>
      <td>42.65</td>
      <td>-73.75</td>
      <td>30.11</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>542</th>
      <td>rawson</td>
      <td>92.0</td>
      <td>ar</td>
      <td>45.0</td>
      <td>-43.30</td>
      <td>-65.11</td>
      <td>62.75</td>
      <td>9.31</td>
    </tr>
    <tr>
      <th>543</th>
      <td>rikitea</td>
      <td>20.0</td>
      <td>pf</td>
      <td>100.0</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>79.04</td>
      <td>4.94</td>
    </tr>
    <tr>
      <th>544</th>
      <td>policoro</td>
      <td>0.0</td>
      <td>it</td>
      <td>93.0</td>
      <td>40.21</td>
      <td>16.68</td>
      <td>45.37</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>545</th>
      <td>khatanga</td>
      <td>48.0</td>
      <td>ru</td>
      <td>92.0</td>
      <td>71.98</td>
      <td>102.47</td>
      <td>-4.04</td>
      <td>4.94</td>
    </tr>
    <tr>
      <th>546</th>
      <td>katsuura</td>
      <td>20.0</td>
      <td>jp</td>
      <td>47.0</td>
      <td>33.93</td>
      <td>134.50</td>
      <td>75.20</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>547</th>
      <td>rikitea</td>
      <td>20.0</td>
      <td>pf</td>
      <td>100.0</td>
      <td>-23.12</td>
      <td>-134.97</td>
      <td>79.04</td>
      <td>4.94</td>
    </tr>
    <tr>
      <th>548</th>
      <td>ulkan</td>
      <td>64.0</td>
      <td>ru</td>
      <td>47.0</td>
      <td>57.24</td>
      <td>107.32</td>
      <td>28.55</td>
      <td>6.51</td>
    </tr>
    <tr>
      <th>549</th>
      <td>tazovskiy</td>
      <td>24.0</td>
      <td>ru</td>
      <td>93.0</td>
      <td>67.47</td>
      <td>78.70</td>
      <td>-7.37</td>
      <td>11.21</td>
    </tr>
  </tbody>
</table>
<p>500 rows × 8 columns</p>
</div>




```python
# Build a scatter plot for temp, hum, cloud, wind vs lat
plt.scatter(weather_data["lat"], weather_data["temp"], marker="o")

# Incorporate the other graph properties
plt.title("Temperature in World Cities")
plt.ylabel("Temperature (Fahrenheit)")
plt.xlabel("Latitude")
plt.grid(True)
plt.xlim(-90, 90)
plt.ylim(-35, 100)

# Save the figure
plt.savefig("TemperatureInWorldCities.png")

# Show plot
plt.show()
```


![png](WeatherPy_files/WeatherPy_13_0.png)



```python
plt.scatter(weather_data["lat"], weather_data["humidity"], marker="o")

# Incorporate the other graph properties
plt.title("Humidity in World Cities")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.grid(True)
plt.xlim(-90, 90)
plt.ylim(0, 110)

# Save the figure
plt.savefig("HumidityInWorldCities.png")

# Show plot
plt.show()
```


![png](WeatherPy_files/WeatherPy_14_0.png)



```python
plt.scatter(weather_data["lat"], weather_data["cloudiness"], marker="o")

# Incorporate the other graph properties
plt.title("Cloudiness in World Cities")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid(True)
plt.xlim(-90, 90)
plt.ylim(-5, 100)

# Save the figure
plt.savefig("CloudinessInWorldCities.png")

# Show plot
plt.show()
```


![png](WeatherPy_files/WeatherPy_15_0.png)



```python
plt.scatter(weather_data["lat"], weather_data["wind speed"], marker="o")

# Incorporate the other graph properties
plt.title("Wind Speed in World Cities")
plt.ylabel("Wind Speed (mph)")
plt.xlabel("Latitude")
plt.grid(True)
plt.xlim(-90, 90)
plt.ylim(0, 60)

# Save the figure
plt.savefig("WindSpeedInWorldCities.png")

# Show plot
plt.show()
```


![png](WeatherPy_files/WeatherPy_16_0.png)



```python
#Soo... the script above runs fine... sometimes. Sometimes, I get a "coord" error in the for loop when I ping 
#the Openweather API, almost as though sometimes the API doesn't recognize some of the cities generated. I tried to 
#write a try/except for this so that it places Nan in the list if the API doesn't find a coordinate for a city, but
#that didn't seem to work for me. I've saved the code here to show that it can and does run... it just doesn't always
#run like this. I'd love to go over how to best debug this.

#Monday after class fix! :)
```


```python
#Observable trends:
#1. The closer the latitude to zero, the higher the temp. (Duh.)
#2. Hmmm... wind speed seems fairly concentrated between 0 and 20 mph, no matter the latitude.
#3. Cloudiness is all over the map...
#4. And humidity seems very high overall, no matter latitude between
```
