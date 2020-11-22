# PyViz-HW #

## This is the PyViz Assignment were we will review housing in San Francisco with some interactive graphing methods using plotly and hvplot! ##

## Here we load relevant libraries and get out API key for mapbox API ## 

```python
import os
import pandas as pd
import matplotlib.pyplot as plt
import hvplot.pandas
import plotly.express as px
from pathlib import Path
from dotenv import load_dotenv

%matplotlib inline

load_dotenv()
mapbox_token = os.getenv("MAPBOX")
px.set_mapbox_access_token(mapbox_token)
print(mapbox_token)
```

## Reading the census data into a Pandas DataFrame ##

```python
file_path = Path("../../HW_6_Pyviz/CSV_s/census_data.csv")
sfo_data = pd.read_csv(file_path, index_col="year")
sfo_data.head()
```
[.](<a href="https://imgur.com/8Auf3h2"><img src="https://i.imgur.com/8Auf3h2.jpg" title="source: imgur.com" /></a>)

## calculate the number of housing units per year and visualize the results as a bar chart using the Pandas plot function. ##

[.](<a href="https://imgur.com/CTbW4Ri"><img src="https://i.imgur.com/CTbW4Ri.jpg" title="source: imgur.com" /></a>)

## Average Gross Rent in San Francisco Per Year ##

[.](<a href="https://imgur.com/xOYAXVY"><img src="https://i.imgur.com/xOYAXVY.jpg" title="source: imgur.com" /></a>)

## Average Prices by Neighborhood ## 

[.](<a href="https://imgur.com/uPQIpWV"><img src="https://i.imgur.com/uPQIpWV.jpg" title="source: imgur.com" /></a>)
[.](<a href="https://imgur.com/974mmwM"><img src="https://i.imgur.com/974mmwM.png" title="source: imgur.com" /></a>)

## The Top 10 Most Expensive Neighborhoods ##

[.](<a href="https://imgur.com/LuSJkKJ"><img src="https://i.imgur.com/LuSJkKJ.png" title="source: imgur.com" /></a>)

## Parallel Categories Analysis ##

```python
px.parallel_categories(
    me,
    dimensions=["sale_price_sqr_foot", "housing_units", "gross_rent"],
    color = 'sale_price_sqr_foot',
    color_continuous_scale=px.colors.sequential.Inferno,
    labels={
        "neighborhood":"neighborhood",
        "sale_price_sqr_foot":"price_per_sqf",
        "housing_units":"units",
        "gross_rent":"rent",
    },
)
```
[.](<a href="https://imgur.com/7w1p0vy"><img src="https://i.imgur.com/7w1p0vy.png" title="source: imgur.com" /></a>)

## MapBox interactive map ##

```python
map = px.scatter_mapbox(
    concat_location,
    lat="Lat",
    lon="Lon",
    size="sale_price_sqr_foot",
    color="Neighborhood",
    zoom=11
)

map.show()
```

[.](<a href="https://imgur.com/3MyJlt5"><img src="https://i.imgur.com/3MyJlt5.png" title="source: imgur.com" /></a>)

## Dashboard within a jupyter notebook ##

```python
def top_most_expensive_neighborhoods():
    """Top 10 Most Expensive Neighborhoods."""
    top_10 = me.hvplot(
        x='neighborhood',
        y='sale_price_sqr_foot',
        kind = 'bar',
        title = 'Top 10 most expensive neighborhoods',
        rot = 90,
        width=600,
        height=600,
        ylim=(600, 1000),
    )
    return top_10
```
 - ^^^ an example of the type of function that must be created to load into the panel
 
 ## Creating the Rows and Columns for our dashboard and launching it ##

```python

Row_1 = pn.Row(average_gross_rent, average_sales_price)
Row_2 = pn.Row(housing_units_per_year)
Yearly_Market_Analysis = pn.Column(Row_1, Row_2)
SF_by_Neighborhood = pn.Column(top_most_expensive_neighborhoods(), average_price_by_neighborhood())
Map_of_SF = pn.Column(neighborhood_map())
Paralell = pn.Row(parallel_coordinates())

SF_Housing = pn.Tabs(
    ('SF from above', Map_of_SF),
    ('Yearly Analysis', Yearly_Market_Analysis),
    ('SF by Neighborhood', SF_by_Neighborhood),
    ('Paralell charts', Paralell),
)
SF_Housing.servable()
panel.servable()
```

[.](<a href="https://imgur.com/HqSCs5U"><img src="https://i.imgur.com/HqSCs5U.gif" title="source: imgur.com" /></a>)
