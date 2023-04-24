# **Transportation Accidents:** Analyzing Deaths and Injuries for Trains and Airplanes

## Team 15 Members:
* Andrea Dukic
* Eric Lim
* Sangeetha Ramesh
* Chaitanya Shekar

## Motivations & Overview
With the East Palestine train derailment in Ohio on February 3, 2023, footage of the chemical burn and its aftermath on the residents and wildlife received significant media attention, lasting over a month since the incident. Considering the media and political stir that followed, we were curious why this incident in particular gained so much publicity.

While considering the derailment in Ohio, we also wondered if other large transportation vehicle accidents occurred in the U.S., which led us to also consider airplane accidents. Globally, airplane incidents seem to catch mainstream headlines quite frequently, from as "minor" as two planes nearly colliding on the runway to as devastating as a 747 crashing.

**--TO DO: add more research questions (if applicable)--**

In both cases, we were interested in understanding the following:
* Did the Ohio train derailment gain so much traction because train derailments in general are exceedingly rare? Or was it because this derailment in particular had noticeably devastating effects? 
* While aircraft accidents seem to occur quite frequently globally, do they really happen that much in the U.S.? Even when they occur, how severe are the accidents?

**--TO DO: add summary of findings--**

## Data Description
* `data/TrainEquipment/*.csv`
  * Equipment safety data from the Federal Railroad Association (FRA) from 1975 to 2022
  * Sources: 
    * https://safetydata.fra.dot.gov/officeofsafety/publicsite/on_the_fly_download.aspx
    * https://railroads.dot.gov/forms-guides-publications/guides/618054-rail-equipment-accidentincident-thru-52011-206kb
* `data/TrainEquipmentAppendix/*.xls`
  * Metadata encoding data that maps railroad accident codes to railroad accident types
  * Sources:
    * https://railroads.dot.gov/forms-guides-publications/guides/appendix-c-train-operation-human-factor
    * https://railroads.dot.gov/forms-guides-publications/guides/appendix-c-signal-and-communication
    * https://railroads.dot.gov/forms-guides-publications/guides/appendix-c-track-roadbed-and-structure
    * https://railroads.dot.gov/forms-guides-publications/guides/appendix-c-mechanical-and-electrical-failures
    * https://railroads.dot.gov/forms-guides-publications/guides/appendix-c-miscellaneous-causes-not-otherwise-listed 
* `data/TrainEquipment_merged.csv`
  * Merged csv file of data in `data/TrainEquipment` subsetted for columns of interest
* `data/TrainEquipmentAppendix_merged.csv`
  * Merged csv file of data in `data/TrainEquipmentAppendix`
* `data/train_final.csv`
  * Merged csv file of `TrainEquipment_merged.csv` and `TrainEquipmentAppendix_merged.csv`
* `data/airplanes.csv`
  * Global airplane crash data from Kaggle (originally from OpenData by Socrata but is no longer publicly available) from 1908 to 2009.
  * Sources:
    * https://www.kaggle.com/datasets/saurograndi/airplane-crashes-since-1908
* `data/airplanes_final.csv`
  * Subsetted csv file of `airplanes.csv` for only airplane crashes in the U.S.
* `data/world_country_and_usa_states_latitude_and_longitude_values.csv`
  * Longitude and latitude data for countries from Kaggle (originally from Google Developers public data)
  * Sources:
    * https://www.kaggle.com/datasets/paultimothymooney/latitude-and-longitude-for-every-country-and-state

## Code Description
* `code/munging.ipynb`
  * **Trains:** Merged FRA safety data from 1975 to 2022 and accident encodings data into one large dataset, subsetted for only columns of interest, and performed additional conversions from encoding to informative values based on documentation.
  * **Airplanes:** Subsetted airplane crash data for only incidents in the U.S. via regex extraction on the `Location` column.
  * Key functions:
    * `merge_csv`: merges FRA safety data and accident encodings data
    * `convert_to_date`: converts string date representation to numeric representation (e.g. 75 ==> 1975)
    * `convert_type`: converts numeric accident type encoding to string representation
    * `convert_state`: converts fips state encoding to string state abbreviation
    * `convert_vis`: converts numeric visibility encoding to string representation
    * `convert_weather`: converts numeric weather encoding to string representation
    * `convert_eq`: converts numeric train equipment type encoding to string representation
    * `convert_track`: converts numeric track type encoding to string representation
    * `state_abbreviation`: converts full state names into state abbreviations
* `code/choropleth.ipynb`
  * **Trains:** Created interactive plotly choropleths for train deaths and injuries from 1975 to 2022. Saved each year for deaths and injuries separately to combine into a gif. Combined train data with long/lat data for U.S. states to generate a folium map with markers of accident locations on a map of the U.S. 
  * **Airplanes:** Created interactive plotly choropleths for airplane ground deaths and aircraft deaths from 1908 to 2009. Saved each year for deaths and injuries separately to combine into a gif. Combined airplane data with long/lat data for U.S. states to generate a folium map with markers of accident locations on a map of the U.S. 
  * Key functions:
    * `create_choropleth`: creates interactive plotly choropleths. Appropriately handles labels and aesthetic encodings based on dataset
    * `create_folium_map`: creates folium maps. Appropriately handles labels and aesthetic encodings based on dataset
    * `create_scattergeo`: creates a scatterplot on the map of the U.S. based on lon/lat values. Icons represent points and are scaled based on specified option. Appropriately handles labels and aesthetic encodings (including icon proportionality) based on dataset
    * `generate_gif`: creates a gif using the figures generated by `create_scattergeo`
* `code/line_charts.ipynb`
  * **Trains:** Created interactive plotly time series line chart for train deaths and injuries from 1975 to 2022. The chart has a slider that enables a subset of the data by date to be visualized. Saved the figure as a png. 
  * **Airplanes:** Created interactive plotly time series line chart for plane deaths onboard and on the gruond from 1908 to 2009. The chart has a slider that enables a subset of the data by date to be visualized. Saved the figure as a png. 
  * Key functions:
    * `create_line_chart`: creates interactive line chart. Appropriately handles labels and aesthetic encodings based on dataset. Includes interactive slider to display subset of line chart. 

* `code/wordcloud.ipynb`
  * **Trains:** Created a wordcloud to visualize the most affected cities in train accidents from 1975 to 2022. Saved the plot as a png. Removed all the stopwords from the dataset, removed space betweenthe cities to avoid double counting, and then created a wordcloud using the wordcloud library.
  * **Airplanes:** Created a wordcloud to visualize the most affected cities from air crash from 1975 to 2022. Saved the plot as a png. Removed all the stopwords from the dataset, removed space betweenthe cities to avoid double counting, and then created a wordcloud using the wordcloud library.
  * Key functions:
    * `states`: Maps each state names to it's corresponding abbreviation
    * `save_imgs`: saves images of wordclouds

* `code/correlation.ipynb`
  * **Trains:**  First created a correlation matrix for the train dataset. Then created a heatmap to visualize the correlation matrix. Saved the plot as a png. Then, created another correlation matrix for most used words in the train accident summary. Then created a heatmap to visualize the correlation matrix with the Damage. Saved the plot as a png. Also, consolidated the plots and ctreated an interactive plotly heatmap for the correlation matrix.
  * **Airplanes:** First created a correlation matrix for the airplane dataset. Then created a heatmap to visualize the correlation matrix. Saved the plot as a png. Then, created another correlation matrix for most used words in the aircrash Description. Then created a heatmap to visualize the correlation matrix with the Fatalities. Saved the plot as a png.Also, consolidated the plots and ctreated an interactive plotly heatmap for the correlation matrix.
  * Key functions:
    * `word_counts.sum`: Calculate the total frequency for each word
    * `pio.write_image`: saves interactive plots to html file
    * `corr` : Calculates the correlation between the columns of a dataframe
  
* `code/Histogram.ipynb`
  * **Trains:** Created 6 interactive histograms using plotly for train accidents in the United States from 1975 to 2022. The histograms provide insights into the number of train accidents by state and year, top causes of train accidents for all states, top causes of train accidents for region, percentage of train accidents by type, number of train accidents by weather condition, and number of train accidents by type of train. Used facet, slider and dropdown menus in Plotly charts which make the charts much more interactive and easier to understand. 
  * **Airplanes:** Created 4 interactive histograms using plotly for airplanes accidents in the United States from 1908 to 2009. The histograms provide insights into the number of airplane accidents by state and year, number of flight crashs by year, top 5 pperators with the most flight crashes, and fatalities in flight crashes. Used facet, slider and dropdown menus in Plotly charts which make the charts much more interactive and easier to understand. 
  * Key functions:
    * `px.histogram` : creates interactive plotly histogram. 
    * `go.bar` : creates interactive plotly bar plot.
    * `write_img`: the function takes a Plotly image as input and converts it to a static image before writing it to a file. 