---
layout: post
title:  "41-Year Rainfall and Temperature Timeseries over Cape Town"
date:   2018-01-20 16:13:10
categories: temperature rainfall climate cape town
permalink: /posts/temperature-rainfall-cape-town
image: "https://github.com/oliverangelil/oliverangelil.github.io/blob/master/photos/blog5_ct_view.jpg?raw=true"
---

The current drought around the Cape Town area got me to do a basic analysis of long-term (1977-2017) rainfall and temperature changes observed from the weather station at Cape Town International Airport. This is the analysis that I have been looking for but have not found. I mainly wanted to know if the recent hot and dry conditions are a consequence of normal year-to-year variability or if there's a long-term trend. There is a cool new [tool](http://www.csag.uct.ac.za/current-seasons-rainfall-in-cape-town/) by the Climate Systems Analysis Group (CSAG) from UCT, but I was looking for a few different types of figures.
Let's look at rainfall first. Looks like there's a long term drying trend of 4mm/year, and it's significant by statisticians standards (the p-value of the slope is less than 0.05):

![ct_airport_pr](https://github.com/oliverangelil/oliverangelil.github.io/blob/master/photos/blog5_cape_town_airport_pr.png?raw=true)

How about average temperature... A significant (p < 0.05) increase in average annual temperature over the last 41 years!

![ct_airport_tas](https://github.com/oliverangelil/oliverangelil.github.io/blob/master/photos/blog5_cape_town_airport_tas.png?raw=true) 

What about extremes... Now the hottest temperature for each year is shown (known as TXx to climate scientists). Again a significant increasing trend in hot extremes.

![ct_airport_tmax](https://github.com/oliverangelil/oliverangelil.github.io/blob/master/photos/blog5_cape_town_airport_tmax.png?raw=true) 

What about cold extremes... Now the coldest temperature of each year is shown (known as TNn)... this time it's not significant, most likely due to an anomalously cold night in the year 2000, which has a large influence on the significance.

![ct_airport_tmin](https://github.com/oliverangelil/oliverangelil.github.io/blob/master/photos/blog5_cape_town_airport_tmin.png?raw=true) 

Conclusions: trends for rainfall are decreasing and trends for temperature are increasing. Annual average temperature and maximum temperatures for each year are increasing significantly (p < 0.05). Can we expect these trends to continue into the future? Extremely likely for temperature, a more in-depth analysis should be done to better understand changes in rainfall (e.g. using multiple stations in the Cape Town region which requires more than a Sunday afternoon).

If you're interested in exploring the data yourself, it can be found [here](ftp://ftp.ncdc.noaa.gov/pub/data/gsod/) (the readme.txt describes the data). There is one ascii file per year for every station. Cape Town International Airport is station number "688160". I downloaded the data for that station only, using the following command in bash: wget -r --no-parent -A '*688160*' ftp://ftp.ncdc.noaa.gov/pub/data/gsod/



