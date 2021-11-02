# sf_real_estate
Modeling San Francisco real estate data via geospatial visualizations to identify potential investment opportunities.

## Technologies

In this project we are utilizing Python 3, Jupyter Lab, and the Pandas Library.  We also leveraged the hvplot library with GeoViews to to produce interactive and geographically specific plots to illustrate our data.  

Pandas library -- Incredibly useful Python library for data science and data analysis  
Jupyter Lab -- Robust environment to be able to view and edit devopment projects in a streamlined system.  
hvPlot -- A high-level plotting API for the PyData ecosystem built on HoloViews.
GeoViews -- GeoViews is a Python library that makes it easy to explore and visualize geographical, meteorological, and oceanographic datasets, such as those used in weather, climate, and remote sensing research.

---

## Installation Guide

* Pandas -- The source code is currently hosted on GitHub at: https://github.com/pandas-dev/pandas

Binary installers for the latest released version are available at the Python Package Index (PyPI) and on Conda.

### conda
`conda install pandas`
### or PyPI
`pip install pandas`

* Jupyter Lab -- 
    [Link for detailed instructions on installing Jupyter Lab here.](https://jupyter.org/install)  
    
*  The PyViz Ecosystem (visualization package that includes GeoViews and hvPlot)  

### conda
`conda install -c pyviz hvplot`
### or PyPI
`pip install pyviz`  

**For more detailed information on pyviz installation and other features, please reference the [pyviz website](https://pyviz.org/)

## Dependencies:  

import pandas as pd  

import hvplot.pandas  

from pathlib import Path  

*hvplot and geoviews are imported via the hvplot.pandas import.

---

## Usage

## The first part of the project imports the SF housing data and assesses the growth of housing units available from the years of 2010 to 2016, the timeframe that we will be utilizing throughout our visualizations.  

```python 

# Create a numerical aggregation that groups the data by the year and then averages the results.
housing_units_by_year = sfo_data_df.groupby('year').mean()

# Plot a chart grouping our housing unit by year dataframe by neighborhood  
housing_units_by_year['housing_units'].hvplot.bar(
    title = 'San Francisco housing units from 2010 to 2016', 
    yformatter = '%.0f', 
    ylim = [365000, 385000],
    xlabel = 'Year',
    ylabel = 'Housing Units')```  
    
![Housing units per year plot]('./Images/zoomed-housing-units-by-year.png')  


## The second part of the project is geared towards assessing the trends of average sales price per sq.ft. in San Francisco vs. the gross rent from the years of 2010 to 2016.

```python  
# Filter the data to groupby the year
prices_square_foot_by_year = sfo_data_df.groupby('year').mean()  
# Drop the housing units column from our dataframe  
prices_square_foot_by_year = prices_square_foot_by_year.drop('housing_units', axis=1)  
# Plot the dataframe to summarize the SF overall market trends  
xlabel = 'Year', 
    ylabel = 'Gross Rent/Sale Prices per sq.ft. by year', 
    legend=True, 
    title = 'Gross rent vs. sales prices per sq.ft. of SF real estate, 2010 to 2016')
```
![SF gross rental income vs. price per sq.ft., 2010 to 2016]('../Images/avg-sale-px-sq-foot-gross-rent.png')  
  

##  Now we drill down to analyze changes in gross rental vs. price per sq.ft. by the neighborhood using an interactive hvplot to allow for neighborhood selection within our chart --  

```python  
# Filter data by year and neighborhood, then drop the 'housing units' column from our DataFrame as before
prices_by_year_by_neighborhood = sfo_data_df.groupby(['year', 'neighborhood']).mean()  

#Initialize our visualization -- notice the 'groupby' parameter that allows for a dropdown box within our chart to select specific neighborhoods to analyze the data from  

prices_by_year_by_neighborhood.hvplot(
    groupby = 'neighborhood',
    xlabel = 'Year',
    ylabel = 'Gross Rent/Sales price per sq.ft',
    title = 'Gross Rent and Sales Prices per sq.ft. in SF 2010-2016, by neighborhood') 
```
![Housing and rental trends for SF by neighborhood]('./Images/pricing-info-by-neighborhood.png')  
  

## Finally we create a geospatial plot that allows us to visualize the market data based on a geographic overlay of San Francisco, modeling both rental income and price per sq.ft. with visualizations that allow for a quick analysis of the specific neighborhoods within the city, as well as the ability to zoom in on certain neighborhoods to further drill down on their details.  

```python  
# Read in the dataset with longitude and latitude coordinates for the neighborhoods in SF --  
neighborhood_locations_df = pd.read_csv(Path('./Resources/neighborhoods_coordinates.csv'), index_col = 'Neighborhood')  

# Concat the location dataframe with our existing dataframe to produce one from which we can generate our plot --  
all_neighborhoods_df = pd.concat(
    [neighborhood_locations_df, all_neighborhood_info_df], 
    axis="columns",
    sort=False
)  

# Generate our geoviews plot --
all_neighborhoods_plot = all_neighborhoods_df.hvplot.points(
    'Lon',
    'Lat',
    legend = True,
    geo = True,
    size = 'sale_price_sqr_foot',
    color = 'gross_rent',
    frame_width = 700,
    frame_height = 500,
    tiles = 'OSM',
    ylabel = 'Latitude',
    xlabel = 'Longitude',
    title = 'San Francisco gross rent and sale prices per sq.ft. by neighborhood (marked by circle size)',
    clabel = 'Gross Rent',
    hover_cols = ['Neighborhood', 'sale_price_sqr_foot', 'gross_rent']
    )  
![Geographic chart our real estate dataframe by neighborhood]('./Images/6-4-geoviews-plot.png')  
```  

### With all of the power and flexibility that the hvplot and geoviews libraries afford us, it is easy to assess our datasets about SF real estate trends to identify investment opportunities not only across the city as a whole but also drilling down to specific neighborhoods, adding robust and flexible visualizations to quickly digest the data in a useful and specific way.



## License

Licensed under the [MIT License](https://github.com/git/git-scm.com/blob/main/MIT-LICENSE.txt)  Copyright 2021 Dave Thomas.

