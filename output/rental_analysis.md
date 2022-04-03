# Toronto Dwellings Analysis

In this assignment, you will perform fundamental analysis for the Toronto dwellings market to allow potential real estate investors to choose rental investment properties.


```python
# imports
import panel as pn
import plotly.express as px
import pandas as pd
import matplotlib.pyplot as plt
import os
from pathlib import Path
from dotenv import load_dotenv
from panel.interact import interact
from panel import widgets
```


```python
# Set up Panel Plotly extension
pn.extension('plotly')


```






```python
# Import hvplot.pandas after pn.extension
# This avoids plotly initialization failure
import hvplot.pandas
```










```python
# Read the Mapbox API key
load_dotenv()
map_box_api = os.getenv("mapbox")
px.set_mapbox_access_token(map_box_api)
```

## Load Data


```python
# Read the census data into a Pandas DataFrame
file_path = Path("Data/toronto_neighbourhoods_census_data.csv")
to_data = pd.read_csv(file_path, index_col="year")
to_data.drop(columns=["average_house_value"],inplace=True)
to_data.head()
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
      <th>neighbourhood</th>
      <th>single_detached_house</th>
      <th>apartment_five_storeys_plus</th>
      <th>movable_dwelling</th>
      <th>semi_detached_house</th>
      <th>row_house</th>
      <th>duplex</th>
      <th>apartment_five_storeys_less</th>
      <th>other_house</th>
      <th>shelter_costs_owned</th>
      <th>shelter_costs_rented</th>
    </tr>
    <tr>
      <th>year</th>
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
      <th>2001</th>
      <td>Agincourt North</td>
      <td>3715</td>
      <td>1480</td>
      <td>0</td>
      <td>1055</td>
      <td>1295</td>
      <td>195</td>
      <td>185</td>
      <td>5</td>
      <td>810</td>
      <td>870</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>Agincourt South-Malvern West</td>
      <td>3250</td>
      <td>1835</td>
      <td>0</td>
      <td>545</td>
      <td>455</td>
      <td>105</td>
      <td>425</td>
      <td>0</td>
      <td>806</td>
      <td>892</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>Alderwood</td>
      <td>3175</td>
      <td>315</td>
      <td>0</td>
      <td>470</td>
      <td>50</td>
      <td>185</td>
      <td>370</td>
      <td>0</td>
      <td>817</td>
      <td>924</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>Annex</td>
      <td>1060</td>
      <td>6090</td>
      <td>5</td>
      <td>1980</td>
      <td>605</td>
      <td>275</td>
      <td>3710</td>
      <td>165</td>
      <td>1027</td>
      <td>1378</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>Banbury-Don Mills</td>
      <td>3615</td>
      <td>4465</td>
      <td>0</td>
      <td>240</td>
      <td>380</td>
      <td>15</td>
      <td>1360</td>
      <td>0</td>
      <td>1007</td>
      <td>1163</td>
    </tr>
  </tbody>
</table>
</div>



- - - 

## Dwelling Types Per Year

In this section, you will calculate the number of dwelling types per year. Visualize the results using bar charts and the Pandas plot function. 

**Hint:** Use the Pandas `groupby` function.



```python
# Calculate the sum number of dwelling types units per year (hint: use groupby)
to_data_per_year = to_data.groupby('year').sum()
to_data_per_year.head()
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
      <th>single_detached_house</th>
      <th>apartment_five_storeys_plus</th>
      <th>movable_dwelling</th>
      <th>semi_detached_house</th>
      <th>row_house</th>
      <th>duplex</th>
      <th>apartment_five_storeys_less</th>
      <th>other_house</th>
      <th>shelter_costs_owned</th>
      <th>shelter_costs_rented</th>
    </tr>
    <tr>
      <th>year</th>
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
      <th>2001</th>
      <td>300930</td>
      <td>355015</td>
      <td>75</td>
      <td>90995</td>
      <td>52355</td>
      <td>23785</td>
      <td>116900</td>
      <td>3040</td>
      <td>118563</td>
      <td>152031</td>
    </tr>
    <tr>
      <th>2006</th>
      <td>266860</td>
      <td>379400</td>
      <td>165</td>
      <td>69430</td>
      <td>54690</td>
      <td>44095</td>
      <td>162850</td>
      <td>1335</td>
      <td>184352</td>
      <td>129558</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>274940</td>
      <td>429220</td>
      <td>100</td>
      <td>72480</td>
      <td>60355</td>
      <td>44750</td>
      <td>163895</td>
      <td>2165</td>
      <td>202750</td>
      <td>142771</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>269680</td>
      <td>493270</td>
      <td>95</td>
      <td>71200</td>
      <td>61565</td>
      <td>48585</td>
      <td>165575</td>
      <td>2845</td>
      <td>246584</td>
      <td>175885</td>
    </tr>
  </tbody>
</table>
</div>



**Optional challenge:** Plot each bar chart in a different color.


```python
dwelling_types  = [x for x in to_data_per_year.columns]
plot = to_data_per_year[dwelling_types[0]].hvplot(kind="bar",yformatter='%f')
for i in range(1,len(dwelling_types)):
    plot = plot * to_data_per_year[dwelling_types[i]].hvplot(kind="bar",yformatter='%f')
```


```python
plot
```


![png](output_Dwellingtypesforyear.png)





```python
# Save the dataframe as a csv file
to_data_per_year.to_csv("Data/Export_to_data_per_year.csv")  
```


```python
# Helper create_bar_chart function
def create_bar_chart(data, title, xlabel, ylabel, color):
    """
    Create a barplot based in the data argument.
    """
    plot = data.hvplot(kind="bar", rot=90,yformatter='%f')
    plot.opts(title=title)
    plot.opts(xlabel=xlabel)
    plot.opts(ylabel=ylabel)
    plot.opts(color=color)
    return plot
    
```


```python

```


```python
# Create a bar chart per year to show the number of dwelling types


# Bar chart for 2001
plots_to_show2001 =  create_bar_chart(to_data_per_year.iloc[0],"Dwelling Types in Toronto in 2001","2001","Dwelling Type Units","blue")

# Bar chart for 2006
plots_to_show2006 =  create_bar_chart(to_data_per_year.iloc[1],"Dwelling Types in Toronto in 2006","2006","Dwelling Type Units","red")


# Bar chart for 2011
plots_to_show2011 =  create_bar_chart(to_data_per_year.iloc[2],"Dwelling Types in Toronto in 2011","2011","Dwelling Type Units","yellow")

# Bar chart for 2016
plots_to_show2016 =  create_bar_chart(to_data_per_year.iloc[3],"Dwelling Types in Toronto in 2016","2016","Dwelling Type Units","pink")

```


```python
pn_yearly_column = pn.Column("## Dwelling Types in Toronto", plots_to_show2001, plots_to_show2006, plots_to_show2011, plots_to_show2016)
pn_yearly_column 

```



![png](output_Dwellingtypesforallyears.png)






- - - 

## Average Monthly Shelter Costs in Toronto Per Year

In this section, you will calculate the average monthly shelter costs for owned and rented dwellings and the average house value for each year. Plot the results as a line chart.

**Optional challenge:** Plot each line chart in a different color.


```python
# Calculate the average monthly shelter costs for owned and rented dwellings
df_monthly_shelter_costs = pd.DataFrame(round(to_data[["shelter_costs_owned","shelter_costs_rented"]].groupby("year").mean(),2))
df_monthly_shelter_costs
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
      <th>shelter_costs_owned</th>
      <th>shelter_costs_rented</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2001</th>
      <td>846.88</td>
      <td>1085.94</td>
    </tr>
    <tr>
      <th>2006</th>
      <td>1316.80</td>
      <td>925.41</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>1448.21</td>
      <td>1019.79</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>1761.31</td>
      <td>1256.32</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Helper create_line_chart function
def create_line_chart(data, title, xlabel, ylabel, color):
    """
    Create a line chart based in the data argument.
    """
    return data.plot(title=title, ylabel=ylabel,xlabel=xlabel,color=color)

```


```python
# Create two line charts, one to plot the monthly shelter costs for owned dwelleing and other for rented dwellings per year

# Line chart for owned dwellings
plot_shelter_costs_owned = create_line_chart(df_monthly_shelter_costs[["shelter_costs_owned"]],"For owned dwellings in Toronto","Average monthly shelter costs","Year", color="Red")

#title="Average monthly shelter costs for owned dwellings in Toronto"
#    elif title=="rented":
#        title="Average monthly shelter costs for rented dwellings in Toronto"

# Line chart for rented dwellings
plot_shelter_costs_rented = create_line_chart(df_monthly_shelter_costs[["shelter_costs_rented"]],"For rented dwellings in Toronto","Average monthly shelter costs","Year", color="Blue")

pn_shelter_costs_column = pn.Column("## Average monthly shelter costs", plot_shelter_costs_owned, plot_shelter_costs_rented)
pn_shelter_costs_column
```


![png](output_22_2.png)
    



    
![png](output_22_3.png)
    


## Average House Value per Year

In this section, you want to determine the average house value per year. An investor may want to understand better the sales price of the rental property over time. For example, a customer will want to know if they should expect an increase or decrease in the property value over time so they can determine how long to hold the rental property. You will visualize the `average_house_value` per year as a bar chart.


```python
# Calculate the average house value per year
to_data = pd.read_csv(file_path, index_col=["year","neighbourhood"])
average_house_value = to_data["average_house_value"].groupby("year").mean()
average_house_value.head()
```




    year
    2001    289882.885714
    2006    424059.664286
    2011    530424.721429
    2016    664068.328571
    Name: average_house_value, dtype: float64




```python
# Plot the average house value per year as a line chart
average_house_value.plot(kind="line", title="Average value of a home in Toronto",xlabel="year",ylabel="Average Home Price (CAD)")

```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fb1130a0a10>




    
![png](output_25_1.png)
    


- - - 

## Average House Value by Neighbourhood

In this section, you will use `hvplot` to create an interactive visualization of the average house value with a dropdown selector for the neighbourhood.

**Hint:** It will be easier to create a new DataFrame from grouping the data and calculating the mean house values for each year and neighbourhood.


```python
# Create a new DataFrame with the mean house values by neighbourhood per year
avg_home_value_per_neighbourhood = to_data["average_house_value"].groupby(by=["year","neighbourhood"]).mean()
avg_home_value_per_neighbourhood.head()
```




    year  neighbourhood               
    2001  Agincourt North                 200388.0
          Agincourt South-Malvern West    203047.0
          Alderwood                       259998.0
          Annex                           453850.0
          Banbury-Don Mills               371864.0
    Name: average_house_value, dtype: float64




```python
# Use hvplot to create an interactive line chart of the average house value per neighbourhood
# The plot should have a dropdown selector for the neighbourhood
avg_home_value_per_neighbourhood.hvplot(kind="line", x="year", y="average_house_value", width=600, 
                         groupby="neighbourhood",widget_location="top_left")
```



![png](output_AverageHouseValuebyNeighbourhood.png)

## Number of Dwelling Types per Year

In this section, you will use `hvplot` to create an interactive visualization of the average number of dwelling types per year with a dropdown selector for the neighbourhood.


```python
# Fetch the data of all dwelling types per year
to_data = pd.read_csv(file_path)
to_data.head()
#removing the average house value for better swelling visulization 
avg_dwelling_types = to_data.drop(columns=["average_house_value"])
```


```python
# Use hvplot to create an interactive bar chart of the number of dwelling types per neighbourhood
# The plot should have a dropdown selector for the neighbourhood
avg_dwelling_types.hvplot(kind="bar", x='year', rot=90 , xlabel="year",ylabel="dwelling type units",groupby="neighbourhood",widget_location="top_left",width=800,height=600)

```

![png](output_NumberofDwellingTypesperYear.png)


- - - 

## The Top 10 Most Expensive Neighbourhoods

In this section, you will need to calculate the house value for each neighbourhood and then sort the values to obtain the top 10 most expensive neighbourhoods on average. Plot the results as a bar chart.


```python
# Getting the data from the top 10 expensive neighbourhoods
to_top_10neighbourhoods=to_data.drop(columns=["year"]).groupby(by=["neighbourhood"]).mean().nlargest(10,"average_house_value")
to_top_10neighbourhoods.head(10)
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
      <th>single_detached_house</th>
      <th>apartment_five_storeys_plus</th>
      <th>movable_dwelling</th>
      <th>semi_detached_house</th>
      <th>row_house</th>
      <th>duplex</th>
      <th>apartment_five_storeys_less</th>
      <th>other_house</th>
      <th>average_house_value</th>
      <th>shelter_costs_owned</th>
      <th>shelter_costs_rented</th>
    </tr>
    <tr>
      <th>neighbourhood</th>
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
      <th>Bridle Path-Sunnybrook-York Mills</th>
      <td>2260.00</td>
      <td>331.25</td>
      <td>0.00</td>
      <td>36.25</td>
      <td>90.00</td>
      <td>25.0</td>
      <td>40.00</td>
      <td>0.00</td>
      <td>1526485.75</td>
      <td>2360.75</td>
      <td>2321.75</td>
    </tr>
    <tr>
      <th>Forest Hill South</th>
      <td>1742.50</td>
      <td>2031.25</td>
      <td>1.25</td>
      <td>61.25</td>
      <td>45.00</td>
      <td>75.0</td>
      <td>1027.50</td>
      <td>3.75</td>
      <td>1195992.50</td>
      <td>1781.00</td>
      <td>1313.75</td>
    </tr>
    <tr>
      <th>Lawrence Park South</th>
      <td>3472.50</td>
      <td>773.75</td>
      <td>0.00</td>
      <td>126.25</td>
      <td>38.75</td>
      <td>225.0</td>
      <td>966.25</td>
      <td>16.25</td>
      <td>1094027.75</td>
      <td>1954.00</td>
      <td>1372.75</td>
    </tr>
    <tr>
      <th>Rosedale-Moore Park</th>
      <td>2498.75</td>
      <td>4641.25</td>
      <td>0.00</td>
      <td>486.25</td>
      <td>245.00</td>
      <td>327.5</td>
      <td>1618.75</td>
      <td>2.50</td>
      <td>1093640.00</td>
      <td>1909.75</td>
      <td>1537.25</td>
    </tr>
    <tr>
      <th>St.Andrew-Windfields</th>
      <td>3225.00</td>
      <td>1670.00</td>
      <td>0.00</td>
      <td>185.00</td>
      <td>552.50</td>
      <td>97.5</td>
      <td>586.25</td>
      <td>5.00</td>
      <td>999107.00</td>
      <td>1880.25</td>
      <td>1384.50</td>
    </tr>
    <tr>
      <th>Casa Loma</th>
      <td>916.25</td>
      <td>2310.00</td>
      <td>0.00</td>
      <td>288.75</td>
      <td>201.25</td>
      <td>162.5</td>
      <td>1192.50</td>
      <td>2.50</td>
      <td>981064.25</td>
      <td>1873.75</td>
      <td>1547.75</td>
    </tr>
    <tr>
      <th>Bedford Park-Nortown</th>
      <td>4865.00</td>
      <td>1981.25</td>
      <td>0.00</td>
      <td>43.75</td>
      <td>57.50</td>
      <td>287.5</td>
      <td>1275.00</td>
      <td>88.75</td>
      <td>930415.25</td>
      <td>1786.75</td>
      <td>1255.00</td>
    </tr>
    <tr>
      <th>Forest Hill North</th>
      <td>1488.75</td>
      <td>3392.50</td>
      <td>0.00</td>
      <td>12.50</td>
      <td>16.25</td>
      <td>82.5</td>
      <td>402.50</td>
      <td>1.25</td>
      <td>851680.50</td>
      <td>1722.75</td>
      <td>1245.50</td>
    </tr>
    <tr>
      <th>Kingsway South</th>
      <td>2326.25</td>
      <td>576.25</td>
      <td>0.00</td>
      <td>66.25</td>
      <td>48.75</td>
      <td>20.0</td>
      <td>336.25</td>
      <td>2.50</td>
      <td>843234.25</td>
      <td>1736.75</td>
      <td>1622.00</td>
    </tr>
    <tr>
      <th>Yonge-St.Clair</th>
      <td>565.00</td>
      <td>3948.75</td>
      <td>0.00</td>
      <td>425.00</td>
      <td>212.50</td>
      <td>172.5</td>
      <td>1308.75</td>
      <td>6.25</td>
      <td>813220.25</td>
      <td>1680.75</td>
      <td>1369.00</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Plotting the data from the top 10 expensive neighbourhoods
to_top_10neighbourhoods.hvplot(kind="bar",x="neighbourhood", y="average_house_value", xlabel="Neighbourhood",ylabel="Average House Value", rot=90 ,width=1000,height=800)
```

![png](output_to_top_10neighbourhoods.png)





- - - 

## Neighbourhood Map

In this section, you will read in neighbourhoods location data and build an interactive map with the average house value per neighbourhood. Use a `scatter_mapbox` from Plotly express to create the visualization. Remember, you will need your Mapbox API key for this.

### Load Location Data


```python
# Load neighbourhoods coordinates data
file_path = Path("Data/toronto_neighbourhoods_coordinates.csv")
df_neighbourhood_locations = pd.read_csv(file_path)
df_neighbourhood_locations.head()
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
      <th>neighbourhood</th>
      <th>lat</th>
      <th>lon</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Agincourt North</td>
      <td>43.805441</td>
      <td>-79.266712</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Agincourt South-Malvern West</td>
      <td>43.788658</td>
      <td>-79.265612</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alderwood</td>
      <td>43.604937</td>
      <td>-79.541611</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Annex</td>
      <td>43.671585</td>
      <td>-79.404001</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Banbury-Don Mills</td>
      <td>43.737657</td>
      <td>-79.349718</td>
    </tr>
  </tbody>
</table>
</div>



### Data Preparation

You will need to join the location data with the mean values per neighbourhood.

1. Calculate the mean values for each neighbourhood.

2. Join the average values with the neighbourhood locations.


```python
# Calculate the mean values for each neighborhood
df_to_avg_house_prices= to_data.drop(columns=["year"]).groupby(by=["neighbourhood"]).mean()
df_to_avg_house_prices.head()
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
      <th>single_detached_house</th>
      <th>apartment_five_storeys_plus</th>
      <th>movable_dwelling</th>
      <th>semi_detached_house</th>
      <th>row_house</th>
      <th>duplex</th>
      <th>apartment_five_storeys_less</th>
      <th>other_house</th>
      <th>average_house_value</th>
      <th>shelter_costs_owned</th>
      <th>shelter_costs_rented</th>
    </tr>
    <tr>
      <th>neighbourhood</th>
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
      <th>Agincourt North</th>
      <td>3435.00</td>
      <td>1947.50</td>
      <td>2.50</td>
      <td>863.75</td>
      <td>1406.25</td>
      <td>512.50</td>
      <td>547.50</td>
      <td>10.00</td>
      <td>329811.5</td>
      <td>1109.00</td>
      <td>983.50</td>
    </tr>
    <tr>
      <th>Agincourt South-Malvern West</th>
      <td>2897.50</td>
      <td>2180.00</td>
      <td>1.25</td>
      <td>375.00</td>
      <td>456.25</td>
      <td>523.75</td>
      <td>628.75</td>
      <td>32.50</td>
      <td>334189.0</td>
      <td>1131.25</td>
      <td>985.00</td>
    </tr>
    <tr>
      <th>Alderwood</th>
      <td>2903.75</td>
      <td>302.50</td>
      <td>1.25</td>
      <td>503.75</td>
      <td>76.25</td>
      <td>302.50</td>
      <td>502.50</td>
      <td>1.25</td>
      <td>427922.5</td>
      <td>1166.75</td>
      <td>1003.25</td>
    </tr>
    <tr>
      <th>Annex</th>
      <td>751.25</td>
      <td>7235.00</td>
      <td>1.25</td>
      <td>1375.00</td>
      <td>613.75</td>
      <td>355.00</td>
      <td>4605.00</td>
      <td>83.75</td>
      <td>746977.0</td>
      <td>1692.75</td>
      <td>1315.25</td>
    </tr>
    <tr>
      <th>Banbury-Don Mills</th>
      <td>3572.50</td>
      <td>5388.75</td>
      <td>1.25</td>
      <td>273.75</td>
      <td>626.25</td>
      <td>32.50</td>
      <td>1340.00</td>
      <td>0.00</td>
      <td>612039.0</td>
      <td>1463.50</td>
      <td>1242.75</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Join the average values with the neighbourhood locations
df_to_avg_house_prices_with_location = pd.merge(df_neighbourhood_locations,df_to_avg_house_prices,on="neighbourhood",how="inner")
#df_to_avg_house_prices_with_location.set_index("neighbourhood", inplace=True)
df_to_avg_house_prices_with_location.head()
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
      <th>neighbourhood</th>
      <th>lat</th>
      <th>lon</th>
      <th>single_detached_house</th>
      <th>apartment_five_storeys_plus</th>
      <th>movable_dwelling</th>
      <th>semi_detached_house</th>
      <th>row_house</th>
      <th>duplex</th>
      <th>apartment_five_storeys_less</th>
      <th>other_house</th>
      <th>average_house_value</th>
      <th>shelter_costs_owned</th>
      <th>shelter_costs_rented</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Agincourt North</td>
      <td>43.805441</td>
      <td>-79.266712</td>
      <td>3435.00</td>
      <td>1947.50</td>
      <td>2.50</td>
      <td>863.75</td>
      <td>1406.25</td>
      <td>512.50</td>
      <td>547.50</td>
      <td>10.00</td>
      <td>329811.5</td>
      <td>1109.00</td>
      <td>983.50</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Agincourt South-Malvern West</td>
      <td>43.788658</td>
      <td>-79.265612</td>
      <td>2897.50</td>
      <td>2180.00</td>
      <td>1.25</td>
      <td>375.00</td>
      <td>456.25</td>
      <td>523.75</td>
      <td>628.75</td>
      <td>32.50</td>
      <td>334189.0</td>
      <td>1131.25</td>
      <td>985.00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Alderwood</td>
      <td>43.604937</td>
      <td>-79.541611</td>
      <td>2903.75</td>
      <td>302.50</td>
      <td>1.25</td>
      <td>503.75</td>
      <td>76.25</td>
      <td>302.50</td>
      <td>502.50</td>
      <td>1.25</td>
      <td>427922.5</td>
      <td>1166.75</td>
      <td>1003.25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Annex</td>
      <td>43.671585</td>
      <td>-79.404001</td>
      <td>751.25</td>
      <td>7235.00</td>
      <td>1.25</td>
      <td>1375.00</td>
      <td>613.75</td>
      <td>355.00</td>
      <td>4605.00</td>
      <td>83.75</td>
      <td>746977.0</td>
      <td>1692.75</td>
      <td>1315.25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Banbury-Don Mills</td>
      <td>43.737657</td>
      <td>-79.349718</td>
      <td>3572.50</td>
      <td>5388.75</td>
      <td>1.25</td>
      <td>273.75</td>
      <td>626.25</td>
      <td>32.50</td>
      <td>1340.00</td>
      <td>0.00</td>
      <td>612039.0</td>
      <td>1463.50</td>
      <td>1242.75</td>
    </tr>
  </tbody>
</table>
</div>



### Mapbox Visualization

Plot the average values per neighbourhood using a Plotly express `scatter_mapbox` visualization.


```python
# Create a scatter mapbox to analyze neighbourhood info
to_neighbourhoods = px.scatter_mapbox(
    df_to_avg_house_prices_with_location,
    lat="lat",
    lon="lon",
    size="average_house_value",
    color="average_house_value",
    color_continuous_scale=px.colors.cyclical.IceFire,
    zoom=10, 
    title="Toronto's neighbourhoods avg. house prices", 
    width=1000,
)

```


```python
map_column = pn.Column("## Scatter Mapbox To analyze neighbourhood info", to_neighbourhoods)
```


```python
map_column
```

![png](output_scatter_mapbox.png)






- - -

## Cost Analysis - Optional Challenge

In this section, you will use Plotly express to a couple of plots that investors can interactively filter and explore various factors related to the house value of the Toronto's neighbourhoods. 

### Create a bar chart row facet to plot the average house values for all Toronto's neighbourhoods per year


```python
# YOUR CODE HERE!
```

### Create a sunburst chart to conduct a costs analysis of most expensive neighbourhoods in Toronto per year


```python
# Fetch the data from all expensive neighbourhoods per year.
# YOUR CODE HERE!
```


```python
# Create the sunburst chart
# YOUR CODE HERE!
```
