# Snowman Capstone
### Introduction
The goal of this project is to predict when and where it would be possible to build a snowman.  There are four notebook files.
Snowman_Prediction.ipynb - This file reads in all of the raw weather station data, filters and cleans the data, builds a temperature and snow depth model for each weather station, and generates predictions.
GeoPandas_with_Date_Picker.ipynb - This file reads in a snowman prediction file and generates an interactive visualization where the user can choose a date and the displayed map updates with the data for the date chosen.
Seasonal_Decomp_and_Model_Dev.ipynb - This file does a 'deep dive' into the data of a single weather station to look at the seasonal characteristics of the data. The development of the models and the choosing of hyperparameters used throughout the project was also done using iterations and variations of this notebook.
Model_Performance.ipynb - This file reads in two subsets of data - actual data for 2024 and predictions for 2024. These sets of data are compared to one another to analyze the performance of the modeling procedure in the Snowman_Prediction notebook.
All of the code within this project was run on my home computer using VS Code. Be sure to have GeoPandas, Ipython, and ipywidgets installed.  A full list of necessary libraries can be found in the requirements.txt file.
### Setup
1. Create a folder (henceforth referred to as the main folder) to place the four notebook files. 
2. Create subfolders in the main folder titled 'AggregatedData', 'CleanedData', 'ForecastDailyData', 'ForecastData', and 'RawData'.
3. Unzip the contents of s_18mr25.zip into the main folder. The .shp file within is used for the geographic visualizations.
### Snowman_Prediction
To run through the entire process yourself, I have set up the code and some files for a small example using the weather stations around the town of Newberry, Michigan.  To do so, unzip the contents of the 'Raw Data - Newberry.zip' file into the 'RawData' folder. These files are the raw data from the NOAA website for the 19 weather stations in the small latitude/longitude box around Newberry.  By the end of the process, only two of these weather stations will remain.

It would take a long time to recreate what I did with the full amount of raw data with almost 130,000 weather stations and modeling for the entire continental US. My full run took just over 22 hours. To recreate this project with exactly the data I used, I made a copy of the full data file on my Google Drive at the following link - https://drive.google.com/file/d/1gIFRhUj4zm_YiC2P5iNu43ozuYS2ib4z/view?usp=sharing.  Alternatively, you can download the full file with the latest data available directly from the NOAA site at the following location - https://www.ncei.noaa.gov/data/daily-summaries/archive/daily-summaries-latest.tar.gz.  This file is over 7Gb and once it is unzipped and unpacked, it expands to 134 gigs.

Before running Snowman_Prediction.ipynb, check the variables section to understand the criteria for what is being run and what you're able to change, should you wish to.  The first section is setting the latitude/longitude box that dictates what geographic region you are looking into. The code is set up for the small box around Newberry, Michigan for a quick run. The coordinates are there, commented out, for the full continental US which I did for this project. In my tests, I had done an intermediate step with a box around Iowa, I've left those coordinates as well.

The next section defines what I used as my criteria for building a snowman - a snow depth of at least four inches and high temperature between 30 and 40 degrees Fahrenheit. Though I never changed these conditions throughout my work on this project, the option is there and easy to adjust.

The final section are variables you shouldn't need to change - folder names, names of files that will be created, and different dates that I used as I analyzed different scenarios. 
### GeoPandas_with_Date_Picker
This program will take a snowman file produced by the Snowman_Prediction notebook and visualize the results on a map.  The first variable points to the 'shape' file for the United States, you shouldn't change this.  The SNOWMAN_FILE variable is currently set to 'Snowman_2020s.csv'. That file can be found on my Google Drive here - https://drive.google.com/file/d/1XevnGJ0pFhuak40QleEnzpOkecDp9M-h/view?usp=sharing - and it contains actual data from 2020-2024 and predicted data from 2025-2029. That is the file I used to generate the images used in my report. Because there is so much data, it can be a bit sluggish to update when choosing a date from the date picker, so please be patient. 

However, if you would like to see the results from your own run of the Snowman_Prediction notebook with just Newberry, the default file name for the output is 'SnowmanData.csv'. Alternatively, I have provided a copy of the resulting Snowman file with only the Newberry data in the repository, it is the 'SnowmanData Newberry.csv' file.  With only two stations, the visualization should respond quickly and this can illustrate the basic idea.  Choose 3/7/25 in the date picker to see where both locations are predicted to be suitable for building a snowman.  Choose 3/8/25 to see where one is suitable and one is not.  Choose 3/14/25 (or later) to see where neither location is suitable.

The RADIUS_IN_KILOMETERS variable defines the size of the area around each weather station. I propose that a 50km radius around a weather station is reasonable for this project, but you can adjust that radius as you like.  The FIG_SIZE variable defines the size of the visualization, adjust as desired.
### Seasonal_Decomp_and_Model_Dev
This program is a deep dive exploration of the weather data from a single weather station. The file with the raw weather data is 'USC00202598test.csv' which is found in the repository.  It should be placed in the main folder, NOT in the RawData folder, so that it is always set apart and available to use for this notebook.  

In this notebook, I explore the seasonality of both temperature and snow depth. This notebook is also where I experimented with different ideas for generating the predictions and where I separately tuned the hyperparameters for the temperature model and the snow depth model. Because there is no timely way to separately tune for thousands of weather stations, I chose this one weather station in Newberry, MI in the hopes that it would be a good baseline representation and that the hyperparameters that are best for this weather station would be good for all of the others. 
### Model_Performance
This notebook requires two files. The first file contains the actual observed values for 'can I build a snowman' for the year 2024.  This can be found in my Google drive at https://drive.google.com/file/d/11WRi9NhEu4KU6Ngu6u3BJlxIk8PY6M_W/view?usp=sharing.

The second file was derived from a run through the entire process where I truncated the observed data to end at 12/31/2023 and tasked the program with predicting the values for 2024. This second file contains the predicted values for 'can I build a snowman' for the year 2024 and can be found in my Google drive at https://drive.google.com/file/d/1mqtNkfshZ2VaAlFS5s29s7JloB_WIbZN/view?usp=sharing.

In this notebook, I combine the data to get statistics about the performance of my models. Finally, there is a visualization to see on the map where and when I have true positives, true negatives, false positives, and false negatives.  2/14/24 is a good date to pick to see a few different things.  In the west, there are a lot of true positives, but almost randomly mixed with some misses, more false positives in the north switching to more false negatives as you go south.  

Then in the northern midwest, a lot of false positives.  Looking up weather records, it appears temperatures were often in the right range, but there just wasn't the expected snow depth at that time in the entire region. 

Finally, in the northeast, a cluster of true positives, but my predictions 'missed' where the some of the snow would be, showing a lot of false positives surrounding the true positives, and then a whole cluster of false negatives a little bit to the south.

