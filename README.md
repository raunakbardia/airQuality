# airQuality

Sources:<br />
Pollution Data - https://app.cpcbccr.com/ccr/#/caaqm-dashboard-all/caaqm-landing/caaqm-comparison-data <br /><br />
India Maps:<br />
Map # 1 - https://github.com/yasersakkaf/Visualize-Women-Harrasment-in-India-using-GeoPandas/tree/master/Igismap<br />
Map # 2 - https://github.com/datameet/maps/tree/master/Districts/Census_2011<br /><br />
Data Visualization:<br />
Reference Project - https://github.com/yasersakkaf/Visualize-Women-Harrasment-in-India-using-GeoPandas/tree/master<br /><br />

Steps<br /><br />

1. Access pollution data from https://app.cpcbccr.com/ccr/#/caaqm-dashboard-all/caaqm-landing/caaqm-comparison-data using automatedAirQualityDataExtraction_CAAQMWebsite.py <br /><br />
	1a. The query sent out by the interface is done using drop down menus to select individual stations and the pollutants <br /><br />
	1b. The Python script reads the names of the filtered cities from the excel file "StationList.xlsx" and sends out a query to the website for all stations of that city. It uses Selenium features to interact with the website. <br /><br />
	1c. After the script is run, I manually selected the dates for which I needed the data, and submitted the request for pollution data. <br /><br />
	1d. The query produces a table with an option to export the data in various formats. The Raw data that is downloaded is saved in "DistrictWiseData.xlsm" workbook in the "RawData" sheet. <br /><br />

2. The raw data includes pollution levels monitores at each station. We converted this into city-level data by aggregating information from all stations within the city. <br /><br />
	2a. This is achieved using an Excel macro. The module is saved in "DistrictWiseData.xlsm" <br /><br />
	2b. Resulting data consists of data for all 365 days of 2019 for 128 cities <br /><br />

3. Next step is to map this data to the ~760 districts of India. <br /><br />
	3a. Firstly, the names and details of each district is extracted from Wikipedia using "districtDataExtraction_SingleWebpage.py" <br /><br />
	3b. By comparing the distance of district headquarters from the 128 cities that have the pollution data, we could identify the districts that are within 200 km. <br /><br />
	3c. This was done using Google Maps API and was implemented by Nidhi Tyagi, the other contributor to this project. <br /><br />
	3d. Finally, we have a mapping, where each district corresponds to either one of the cities for which we have extracted the pollution data, or it is identified as No Data Available. <br /><br />

4. This mapping is used to populate the pollution levels for each district and then assign a color for that particular pollution level based on the CPCB standards established here: https://app.cpcbccr.com/ccr_docs/FINAL-REPORT_AQI_.pdf <br /><br />

5. Next step is the visualization done using: Visualization_Final.ipynb <br /><br />
	5a. This step largely followed the scripts developed by Yaser Sakkaf. His github repo link is here: https://github.com/yasersakkaf/Visualize-Women-Harrasment-in-India-using-GeoPandas and the corresponding article is here: https://towardsdatascience.com/visualizing-map-of-crime-against-women-in-india-using-geopandas-2d31af1a369b
The github profile also had shape files for district level india boundaries - MAP #1. <br /><br />
	5b. In my case, we first matched all the district names between the geo shape file for India and the district names in our pollution data. This was the most arduous task as it was largely manual. I have a greater appreciation for the problems arising out of entity-matching in data science. <br /><br />
	5c. Another hassle was that most shape files of India available online (https://www.igismap.com/) do not show the J & K and Ladakh region appropriately. Finally, I got the correct shape files here: https://github.com/datameet/maps/tree/master/Districts/Census_2011 - MAP #2. But given the recent distinction between Andhra Pradesh and Telangana, even these files were not entirely correct. <br /><br />
	5d. Hence, I overlaid MAP #2 with the more recent MAP #1, which we could correlate with the pollution data. Regardless though, we do not have any air quality monitoring stations in the entire area of Jammu & Kashmir and Ladakh. <br /><br />
	5e. Finally, after matching the district names with Map #1, we merged it with pollution data set for all 365 days separately in a for loop to create 365 images each of PM2.5, PM10 and Ozone levels <br /><br />

6. Converting it to video <br /><br />
	6a. Using the ffmpeg project, the sequence of images could easily be converted into a video. As the image sizes reached a totla of about 1GB, I have only uploaded the videos for reference. <br /><br />
	6b. The exact command was: ffmpeg -framerate 12 -i "Day %d.png" Ozone.avi 
