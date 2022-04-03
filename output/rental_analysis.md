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




<div id='1724'>





  <div class="bk-root" id="0aa946f3-45c3-4c42-852f-5522d9a5f028" data-root-id="1724"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"f31813f8-60c1-4662-999b-e87aa7f2093f":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"axis_label":"2001","coordinates":null,"formatter":{"id":"1769"},"group":null,"major_label_orientation":1.5707963267948966,"major_label_policy":{"id":"1770"},"ticker":{"id":"1739"}},"id":"1738","type":"CategoricalAxis"},{"attributes":{},"id":"1977","type":"AllLabels"},{"attributes":{},"id":"1739","type":"CategoricalTicker"},{"attributes":{"source":{"id":"1962"}},"id":"1969","type":"CDSView"},{"attributes":{"bottom":{"value":0},"fill_alpha":{"value":1.0},"fill_color":{"value":"pink"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"pink"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"top":{"field":"A_2016"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1970","type":"VBar"},{"attributes":{"axis":{"id":"1809"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1812","type":"Grid"},{"attributes":{},"id":"1974","type":"AllLabels"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"yellow"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"yellow"},"line_alpha":{"value":0.2},"top":{"field":"A_2011"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1899","type":"VBar"},{"attributes":{},"id":"1963","type":"Selection"},{"attributes":{"bottom":{"value":0},"fill_alpha":{"value":1.0},"fill_color":{"value":"yellow"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"yellow"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"top":{"field":"A_2011"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1902","type":"VBar"},{"attributes":{},"id":"1986","type":"UnionRenderers"},{"attributes":{"callback":null,"renderers":[{"id":"1968"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["2016","@{A_2016}"]]},"id":"1932","type":"HoverTool"},{"attributes":{"below":[{"id":"1942"}],"center":[{"id":"1944"},{"id":"1948"}],"height":300,"left":[{"id":"1945"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1968"}],"sizing_mode":"fixed","title":{"id":"1934"},"toolbar":{"id":"1955"},"width":700,"x_range":{"id":"1726"},"x_scale":{"id":"1938"},"y_range":{"id":"1931"},"y_scale":{"id":"1940"}},"id":"1933","subtype":"Figure","type":"Plot"},{"attributes":{"format":"%f"},"id":"1903","type":"PrintfTickFormatter"},{"attributes":{"coordinates":null,"group":null,"text":"Dwelling Types in Toronto in 2016","text_color":"black","text_font_size":"12pt"},"id":"1934","type":"Title"},{"attributes":{},"id":"1906","type":"AllLabels"},{"attributes":{},"id":"1940","type":"LinearScale"},{"attributes":{"data":{"A_2016":[269680,493270,95,71200,61565,48585,165575,2845,246584,175885],"index":["single_detached_house","apartment_five_storeys_plus","movable_dwelling","semi_detached_house","row_house","duplex","apartment_five_storeys_less","other_house","shelter_costs_owned","shelter_costs_rented"]},"selected":{"id":"1963"},"selection_policy":{"id":"1986"}},"id":"1962","type":"ColumnDataSource"},{"attributes":{"axis":{"id":"1945"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1948","type":"Grid"},{"attributes":{"data":{"A_2001":[300930,355015,75,90995,52355,23785,116900,3040,118563,152031],"index":["single_detached_house","apartment_five_storeys_plus","movable_dwelling","semi_detached_house","row_house","duplex","apartment_five_storeys_less","other_house","shelter_costs_owned","shelter_costs_rented"]},"selected":{"id":"1759"},"selection_policy":{"id":"1782"}},"id":"1758","type":"ColumnDataSource"},{"attributes":{"source":{"id":"1894"}},"id":"1901","type":"CDSView"},{"attributes":{},"id":"1905","type":"CategoricalTickFormatter"},{"attributes":{},"id":"1909","type":"AllLabels"},{"attributes":{},"id":"1938","type":"CategoricalScale"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"red"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"red"},"line_alpha":{"value":0.2},"top":{"field":"A_2006"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1831","type":"VBar"},{"attributes":{"coordinates":null,"data_source":{"id":"1826"},"glyph":{"id":"1829"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1831"},"nonselection_glyph":{"id":"1830"},"selection_glyph":{"id":"1834"},"view":{"id":"1833"}},"id":"1832","type":"GlyphRenderer"},{"attributes":{"axis_label":"2016","coordinates":null,"formatter":{"id":"1973"},"group":null,"major_label_orientation":1.5707963267948966,"major_label_policy":{"id":"1974"},"ticker":{"id":"1943"}},"id":"1942","type":"CategoricalAxis"},{"attributes":{},"id":"1759","type":"Selection"},{"attributes":{"end":472132.0,"reset_end":472132.0,"reset_start":0.0,"tags":[[["2011","2011",null]]]},"id":"1863","type":"Range1d"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"red"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"red"},"line_alpha":{"value":0.1},"top":{"field":"A_2006"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1830","type":"VBar"},{"attributes":{},"id":"1943","type":"CategoricalTicker"},{"attributes":{"axis":{"id":"1806"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1808","type":"Grid"},{"attributes":{"axis":{"id":"1942"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1944","type":"Grid"},{"attributes":{},"id":"1773","type":"AllLabels"},{"attributes":{},"id":"1813","type":"SaveTool"},{"attributes":{},"id":"1946","type":"BasicTicker"},{"attributes":{},"id":"1918","type":"UnionRenderers"},{"attributes":{"axis_label":"Dwelling Type Units","coordinates":null,"formatter":{"id":"1971"},"group":null,"major_label_policy":{"id":"1977"},"ticker":{"id":"1946"}},"id":"1945","type":"LinearAxis"},{"attributes":{"fill_color":{"value":"blue"},"hatch_color":{"value":"blue"},"top":{"field":"A_2001"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1761","type":"VBar"},{"attributes":{},"id":"1810","type":"BasicTicker"},{"attributes":{"axis":{"id":"1877"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1880","type":"Grid"},{"attributes":{"source":{"id":"1826"}},"id":"1833","type":"CDSView"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"pink"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"pink"},"line_alpha":{"value":0.1},"top":{"field":"A_2016"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1966","type":"VBar"},{"attributes":{},"id":"1814","type":"PanTool"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"pink"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"pink"},"line_alpha":{"value":0.2},"top":{"field":"A_2016"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1967","type":"VBar"},{"attributes":{},"id":"1973","type":"CategoricalTickFormatter"},{"attributes":{"below":[{"id":"1874"}],"center":[{"id":"1876"},{"id":"1880"}],"height":300,"left":[{"id":"1877"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1900"}],"sizing_mode":"fixed","title":{"id":"1866"},"toolbar":{"id":"1887"},"width":700,"x_range":{"id":"1726"},"x_scale":{"id":"1870"},"y_range":{"id":"1863"},"y_scale":{"id":"1872"}},"id":"1865","subtype":"Figure","type":"Plot"},{"attributes":{"axis_label":"Dwelling Type Units","coordinates":null,"formatter":{"id":"1835"},"group":null,"major_label_policy":{"id":"1841"},"ticker":{"id":"1810"}},"id":"1809","type":"LinearAxis"},{"attributes":{"coordinates":null,"data_source":{"id":"1962"},"glyph":{"id":"1965"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1967"},"nonselection_glyph":{"id":"1966"},"selection_glyph":{"id":"1970"},"view":{"id":"1969"}},"id":"1968","type":"GlyphRenderer"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"blue"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"blue"},"line_alpha":{"value":0.1},"top":{"field":"A_2001"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1762","type":"VBar"},{"attributes":{"coordinates":null,"data_source":{"id":"1758"},"glyph":{"id":"1761"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1763"},"nonselection_glyph":{"id":"1762"},"selection_glyph":{"id":"1766"},"view":{"id":"1765"}},"id":"1764","type":"GlyphRenderer"},{"attributes":{},"id":"1949","type":"SaveTool"},{"attributes":{"data":{"A_2006":[266860,379400,165,69430,54690,44095,162850,1335,184352,129558],"index":["single_detached_house","apartment_five_storeys_plus","movable_dwelling","semi_detached_house","row_house","duplex","apartment_five_storeys_less","other_house","shelter_costs_owned","shelter_costs_rented"]},"selected":{"id":"1827"},"selection_policy":{"id":"1850"}},"id":"1826","type":"ColumnDataSource"},{"attributes":{},"id":"1950","type":"PanTool"},{"attributes":{},"id":"1782","type":"UnionRenderers"},{"attributes":{"end":542587.5,"reset_end":542587.5,"reset_start":0.0,"tags":[[["2016","2016",null]]]},"id":"1931","type":"Range1d"},{"attributes":{},"id":"1951","type":"WheelZoomTool"},{"attributes":{"tools":[{"id":"1932"},{"id":"1949"},{"id":"1950"},{"id":"1951"},{"id":"1952"},{"id":"1953"}]},"id":"1955","type":"Toolbar"},{"attributes":{"overlay":{"id":"1954"}},"id":"1952","type":"BoxZoomTool"},{"attributes":{"factors":["single_detached_house","apartment_five_storeys_plus","movable_dwelling","semi_detached_house","row_house","duplex","apartment_five_storeys_less","other_house","shelter_costs_owned","shelter_costs_rented"],"tags":[[["index","index",null]]]},"id":"1726","type":"FactorRange"},{"attributes":{"fill_alpha":{"value":0.2},"fill_color":{"value":"blue"},"hatch_alpha":{"value":0.2},"hatch_color":{"value":"blue"},"line_alpha":{"value":0.2},"top":{"field":"A_2001"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1763","type":"VBar"},{"attributes":{},"id":"1742","type":"BasicTicker"},{"attributes":{"callback":null,"renderers":[{"id":"1900"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["2011","@{A_2011}"]]},"id":"1864","type":"HoverTool"},{"attributes":{"callback":null,"renderers":[{"id":"1764"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["2001","@{A_2001}"]]},"id":"1728","type":"HoverTool"},{"attributes":{},"id":"1953","type":"ResetTool"},{"attributes":{"end":390509.0,"reset_end":390509.0,"reset_start":0.0,"tags":[[["2001","2001",null]]]},"id":"1727","type":"Range1d"},{"attributes":{"css_classes":["markdown"],"margin":[5,5,5,5],"name":"Markdown04599","text":"&lt;h2&gt;Dwelling Types in Toronto&lt;/h2&gt;"},"id":"1725","type":"panel.models.markup.HTML"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1954","type":"BoxAnnotation"},{"attributes":{"below":[{"id":"1738"}],"center":[{"id":"1740"},{"id":"1744"}],"height":300,"left":[{"id":"1741"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1764"}],"sizing_mode":"fixed","title":{"id":"1730"},"toolbar":{"id":"1751"},"width":700,"x_range":{"id":"1726"},"x_scale":{"id":"1734"},"y_range":{"id":"1727"},"y_scale":{"id":"1736"}},"id":"1729","subtype":"Figure","type":"Plot"},{"attributes":{"fill_color":{"value":"pink"},"hatch_color":{"value":"pink"},"top":{"field":"A_2016"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1965","type":"VBar"},{"attributes":{},"id":"1734","type":"CategoricalScale"},{"attributes":{"fill_alpha":{"value":0.1},"fill_color":{"value":"yellow"},"hatch_alpha":{"value":0.1},"hatch_color":{"value":"yellow"},"line_alpha":{"value":0.1},"top":{"field":"A_2011"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1898","type":"VBar"},{"attributes":{"coordinates":null,"group":null,"text":"Dwelling Types in Toronto in 2001","text_color":"black","text_font_size":"12pt"},"id":"1730","type":"Title"},{"attributes":{"axis":{"id":"1738"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1740","type":"Grid"},{"attributes":{"axis":{"id":"1741"},"coordinates":null,"dimension":1,"grid_line_color":null,"group":null,"ticker":null},"id":"1744","type":"Grid"},{"attributes":{},"id":"1872","type":"LinearScale"},{"attributes":{"client_comm_id":"e44d2f2068324a38a50a6623a7a0f92c","comm_id":"dfc8a9df133841458112884954f3e382","plot_id":"1724"},"id":"2106","type":"panel.models.comm_manager.CommManager"},{"attributes":{"format":"%f"},"id":"1971","type":"PrintfTickFormatter"},{"attributes":{},"id":"1736","type":"LinearScale"},{"attributes":{},"id":"1746","type":"PanTool"},{"attributes":{"format":"%f"},"id":"1835","type":"PrintfTickFormatter"},{"attributes":{"overlay":{"id":"1818"}},"id":"1816","type":"BoxZoomTool"},{"attributes":{},"id":"1870","type":"CategoricalScale"},{"attributes":{"bottom":{"value":0},"fill_alpha":{"value":1.0},"fill_color":{"value":"blue"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"blue"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"top":{"field":"A_2001"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1766","type":"VBar"},{"attributes":{},"id":"1745","type":"SaveTool"},{"attributes":{},"id":"1838","type":"AllLabels"},{"attributes":{"axis_label":"2011","coordinates":null,"formatter":{"id":"1905"},"group":null,"major_label_orientation":1.5707963267948966,"major_label_policy":{"id":"1906"},"ticker":{"id":"1875"}},"id":"1874","type":"CategoricalAxis"},{"attributes":{},"id":"1837","type":"CategoricalTickFormatter"},{"attributes":{"end":417323.5,"reset_end":417323.5,"reset_start":0.0,"tags":[[["2006","2006",null]]]},"id":"1795","type":"Range1d"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1886","type":"BoxAnnotation"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1818","type":"BoxAnnotation"},{"attributes":{"tools":[{"id":"1728"},{"id":"1745"},{"id":"1746"},{"id":"1747"},{"id":"1748"},{"id":"1749"}]},"id":"1751","type":"Toolbar"},{"attributes":{},"id":"1747","type":"WheelZoomTool"},{"attributes":{"bottom":{"value":0},"fill_alpha":{"value":1.0},"fill_color":{"value":"red"},"hatch_alpha":{"value":1.0},"hatch_color":{"value":"red"},"hatch_scale":{"value":12.0},"hatch_weight":{"value":1.0},"line_alpha":{"value":1.0},"line_cap":{"value":"butt"},"line_color":{"value":"black"},"line_dash":{"value":[]},"line_dash_offset":{"value":0},"line_join":{"value":"bevel"},"line_width":{"value":1},"top":{"field":"A_2006"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1834","type":"VBar"},{"attributes":{"bottom_units":"screen","coordinates":null,"fill_alpha":0.5,"fill_color":"lightgrey","group":null,"left_units":"screen","level":"overlay","line_alpha":1.0,"line_color":"black","line_dash":[4,4],"line_width":2,"right_units":"screen","syncable":false,"top_units":"screen"},"id":"1750","type":"BoxAnnotation"},{"attributes":{},"id":"1850","type":"UnionRenderers"},{"attributes":{},"id":"1769","type":"CategoricalTickFormatter"},{"attributes":{"coordinates":null,"group":null,"text":"Dwelling Types in Toronto in 2011","text_color":"black","text_font_size":"12pt"},"id":"1866","type":"Title"},{"attributes":{"fill_color":{"value":"red"},"hatch_color":{"value":"red"},"top":{"field":"A_2006"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1829","type":"VBar"},{"attributes":{"source":{"id":"1758"}},"id":"1765","type":"CDSView"},{"attributes":{},"id":"1827","type":"Selection"},{"attributes":{},"id":"1802","type":"CategoricalScale"},{"attributes":{"overlay":{"id":"1750"}},"id":"1748","type":"BoxZoomTool"},{"attributes":{},"id":"1885","type":"ResetTool"},{"attributes":{},"id":"1875","type":"CategoricalTicker"},{"attributes":{},"id":"1749","type":"ResetTool"},{"attributes":{},"id":"1770","type":"AllLabels"},{"attributes":{},"id":"1882","type":"PanTool"},{"attributes":{"coordinates":null,"data_source":{"id":"1894"},"glyph":{"id":"1897"},"group":null,"hover_glyph":null,"muted_glyph":{"id":"1899"},"nonselection_glyph":{"id":"1898"},"selection_glyph":{"id":"1902"},"view":{"id":"1901"}},"id":"1900","type":"GlyphRenderer"},{"attributes":{"overlay":{"id":"1886"}},"id":"1884","type":"BoxZoomTool"},{"attributes":{"children":[{"id":"1725"},{"id":"1729"},{"id":"1797"},{"id":"1865"},{"id":"1933"}],"margin":[0,0,0,0],"name":"Column04621"},"id":"1724","type":"Column"},{"attributes":{},"id":"1817","type":"ResetTool"},{"attributes":{"fill_color":{"value":"yellow"},"hatch_color":{"value":"yellow"},"top":{"field":"A_2011"},"width":{"value":0.8},"x":{"field":"index"}},"id":"1897","type":"VBar"},{"attributes":{"axis_label":"Dwelling Type Units","coordinates":null,"formatter":{"id":"1767"},"group":null,"major_label_policy":{"id":"1773"},"ticker":{"id":"1742"}},"id":"1741","type":"LinearAxis"},{"attributes":{"data":{"A_2011":[274940,429220,100,72480,60355,44750,163895,2165,202750,142771],"index":["single_detached_house","apartment_five_storeys_plus","movable_dwelling","semi_detached_house","row_house","duplex","apartment_five_storeys_less","other_house","shelter_costs_owned","shelter_costs_rented"]},"selected":{"id":"1895"},"selection_policy":{"id":"1918"}},"id":"1894","type":"ColumnDataSource"},{"attributes":{},"id":"1883","type":"WheelZoomTool"},{"attributes":{"coordinates":null,"group":null,"text":"Dwelling Types in Toronto in 2006","text_color":"black","text_font_size":"12pt"},"id":"1798","type":"Title"},{"attributes":{"format":"%f"},"id":"1767","type":"PrintfTickFormatter"},{"attributes":{"tools":[{"id":"1864"},{"id":"1881"},{"id":"1882"},{"id":"1883"},{"id":"1884"},{"id":"1885"}]},"id":"1887","type":"Toolbar"},{"attributes":{},"id":"1807","type":"CategoricalTicker"},{"attributes":{},"id":"1881","type":"SaveTool"},{"attributes":{"axis_label":"2006","coordinates":null,"formatter":{"id":"1837"},"group":null,"major_label_orientation":1.5707963267948966,"major_label_policy":{"id":"1838"},"ticker":{"id":"1807"}},"id":"1806","type":"CategoricalAxis"},{"attributes":{},"id":"1815","type":"WheelZoomTool"},{"attributes":{},"id":"1841","type":"AllLabels"},{"attributes":{},"id":"1804","type":"LinearScale"},{"attributes":{"below":[{"id":"1806"}],"center":[{"id":"1808"},{"id":"1812"}],"height":300,"left":[{"id":"1809"}],"margin":[5,5,5,5],"min_border_bottom":10,"min_border_left":10,"min_border_right":10,"min_border_top":10,"renderers":[{"id":"1832"}],"sizing_mode":"fixed","title":{"id":"1798"},"toolbar":{"id":"1819"},"width":700,"x_range":{"id":"1726"},"x_scale":{"id":"1802"},"y_range":{"id":"1795"},"y_scale":{"id":"1804"}},"id":"1797","subtype":"Figure","type":"Plot"},{"attributes":{"tools":[{"id":"1796"},{"id":"1813"},{"id":"1814"},{"id":"1815"},{"id":"1816"},{"id":"1817"}]},"id":"1819","type":"Toolbar"},{"attributes":{"axis":{"id":"1874"},"coordinates":null,"grid_line_color":null,"group":null,"ticker":null},"id":"1876","type":"Grid"},{"attributes":{"callback":null,"renderers":[{"id":"1832"}],"tags":["hv_created"],"tooltips":[["index","@{index}"],["2006","@{A_2006}"]]},"id":"1796","type":"HoverTool"},{"attributes":{},"id":"1878","type":"BasicTicker"},{"attributes":{},"id":"1895","type":"Selection"},{"attributes":{"axis_label":"Dwelling Type Units","coordinates":null,"formatter":{"id":"1903"},"group":null,"major_label_policy":{"id":"1909"},"ticker":{"id":"1878"}},"id":"1877","type":"LinearAxis"}],"root_ids":["1724","2106"]},"title":"Bokeh Application","version":"2.4.1"}};
    var render_items = [{"docid":"f31813f8-60c1-4662-999b-e87aa7f2093f","root_ids":["1724"],"roots":{"1724":"0aa946f3-45c3-4c42-852f-5522d9a5f028"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined && ( root['Plotly'] !== undefined)) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined && (root['Plotly'] !== undefined)) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>



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






<div id='2287'>





  <div class="bk-root" id="5e443710-80ab-4b99-9364-3ab50db06a20" data-root-id="2287"></div>
</div>
<script type="application/javascript">(function(root) {
  function embed_document(root) {
    var docs_json = {"2d821a06-5fd5-4990-acb8-5a429fe41660":{"defs":[{"extends":null,"module":null,"name":"ReactiveHTML1","overrides":[],"properties":[]},{"extends":null,"module":null,"name":"FlexBox1","overrides":[],"properties":[{"default":"flex-start","kind":null,"name":"align_content"},{"default":"flex-start","kind":null,"name":"align_items"},{"default":"row","kind":null,"name":"flex_direction"},{"default":"wrap","kind":null,"name":"flex_wrap"},{"default":"flex-start","kind":null,"name":"justify_content"}]},{"extends":null,"module":null,"name":"TemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]},{"extends":null,"module":null,"name":"MaterialTemplateActions1","overrides":[],"properties":[{"default":0,"kind":null,"name":"open_modal"},{"default":0,"kind":null,"name":"close_modal"}]}],"roots":{"references":[{"attributes":{"client_comm_id":"cc025efba49e49e6ac63d05c1442c579","comm_id":"c6b361e5b93041c0a903380222949aa2","plot_id":"2287"},"id":"2291","type":"panel.models.comm_manager.CommManager"},{"attributes":{"children":[{"id":"2288"},{"id":"2289"},{"id":"2290"}],"margin":[0,0,0,0],"name":"Column04905"},"id":"2287","type":"Column"},{"attributes":{"margin":[5,5,5,5],"name":"Str04901","text":"&lt;pre&gt;AxesSubplot(0.125,0.125;0.775x0.755)&lt;/pre&gt;"},"id":"2289","type":"panel.models.markup.HTML"},{"attributes":{"margin":[5,5,5,5],"name":"Str04903","text":"&lt;pre&gt;AxesSubplot(0.125,0.125;0.775x0.755)&lt;/pre&gt;"},"id":"2290","type":"panel.models.markup.HTML"},{"attributes":{"css_classes":["markdown"],"margin":[5,5,5,5],"name":"Markdown04899","text":"&lt;h2&gt;Average monthly shelter costs&lt;/h2&gt;"},"id":"2288","type":"panel.models.markup.HTML"}],"root_ids":["2287","2291"]},"title":"Bokeh Application","version":"2.4.1"}};
    var render_items = [{"docid":"2d821a06-5fd5-4990-acb8-5a429fe41660","root_ids":["2287"],"roots":{"2287":"5e443710-80ab-4b99-9364-3ab50db06a20"}}];
    root.Bokeh.embed.embed_items_notebook(docs_json, render_items);
  }
  if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined && ( root['Plotly'] !== undefined)) {
    embed_document(root);
  } else {
    var attempts = 0;
    var timer = setInterval(function(root) {
      if (root.Bokeh !== undefined && root.Bokeh.Panel !== undefined && (root['Plotly'] !== undefined)) {
        clearInterval(timer);
        embed_document(root);
      } else if (document.readyState == "complete") {
        attempts++;
        if (attempts > 200) {
          clearInterval(timer);
          console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing");
        }
      }
    }, 25, root)
  }
})(window);</script>




    
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
