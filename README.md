# **Transportation Accidents:** Analyzing Deaths and Injuries for Trains and Airplanes

## Team 15 Members:
* Andrea Dukic
* Eric Lim
* Sangeetha Ramesh
* Chaitanya Shekar

## Motivations & Overview
With the East Palestine train derailment in Ohio on February 3, 2023, footage of the chemical burn and its aftermath on the residents and wildlife received significant media attention, lasting over a month since the incident. Considering the media and political stir that followed, we were curious why this incident in particular gained so much publicity.

While considering the derailment in Ohio, we also wondered if other large transportation vehicle accidents occurred in the U.S., which led us to also consider airplane accidents. Globally, airplane incidents seem to catch mainstream headlines quite frequently, from as "minor" as two planes nearly colliding on the runway to as devastating as a 747 crashing.

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
    * `save_imgs`: saves images of each year for plotly choropleths
    * `create_gif`: converts images in a specified directory into a gif
    * `create_folium_map`: creates folium maps. Appropriately handles labels and aesthetic encodings based on dataset
* Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.


--------------------------------------- DELETE BELOW ONCE DONE --------------------------------------------------------
# ANLY 503 Project Repo

This is the team repository you will be use for your 503 project. All your team's work will happen here. 

Links of interest:
* The project requirements are in the [`instructions.md`](instructions.md) document
* The repository structure is described in the [reposity structure section](#repository-structure) below
* You **will** make changes to this `README.md` file within your repository. These changes are descripned in the [instructions section](#instructions-for-modifying-this-readmemd-file) below.

## Repository structure

You will work within an **organized** repository and apply coding and development best practices. The repository has the following structure:

```.
├── README.md
├── code/
├── data/
├── img/
└── website/
```

* The `code/` directory is where you will develop all your code.  You may add additional sub-directories as needed to modularize your development.

* The `data/` directory should contain your data files and should have multiple sub-directories (i.e. raw, processed, analytical, etc.) as needed.

* The `img/` directory should contain any external images that you need for your site. However, all your viz's should be generated programmatically in your source code.

* The `website/` directory where the website will be deployed. It must be self-contained and accessible via an index.html within this sub-directory.  Any website asset (images, html, css, JavaScript source code) must be added to this directory. 

There is an empty placeholder file in each subdirectory called `placeholder-to-be-deleted.txt`. This file may be deleted **after** you save other files in those subdirectories. This file is needed to be able to keep the empty directory in the repo.

Other files we expect to see at the top level of this repo may include:
- `.gitignore`


## Instructions for modifying this `README.md` file

The README.md file in a repository usually contains additional information about your project. Currently this file contains information about the repository structure. However, for the wip and final submissions, you will make the following changes to this file:

* You can delete the current content of the `README.md` file
* Add a project title section
* Add your team section with your team number and team member names
* Add an executive summary section describing your project
* Provide a description of all your code files, datasets, etc.

