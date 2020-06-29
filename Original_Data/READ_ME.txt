##########################
####  Data downloaded using noaaweatherdataGUI.py.
##########################
CBEP uses a custom Python program to download data from NOAA's online data repositories.  Specifically, data were accessed through NOAA's National Centers for Environnmental Information.

Here, we have downloaded daily (GHCND) , monthly (GSOM), and annual (GSOY) weather summaries via API v2. Information on this API is available here:  https://www.ncdc.noaa.gov/cdo-web/webservices/v2

Documentation on specific datasets is available at
https://www.ncdc.noaa.gov/cdo-web/datasets

##########################
#### Software Access
##########################
CBEP's python program is available in this repository. The program was originally developed in Python 2.x, but has been converted for Python 3.x.  This is not a polished programs, and may or may not work "out of the box." Some functionality implied by the User Interface is incomplete.

To use the program, you will need to get an access token from NOAA (see the website describing the API, above), and modify the code slightly on line 43 to set MYTOKEN equal to the value of the token you receive from NOAA.

The "wide" data formatter makes some assumptions about the data being downloaded that does not hold for older data (principally that the data categories in the first portion of the downloaded data represent all possible categories.  It is thus safest with the daily data to download the data on "long" format, unless you are downloading for relatively short periods of time when data categories remain consistent.

The program is fairly slow for long data series, since  it often submits HTTP requests for data on monthly intervals.  In our experience, each request takes from three to thirty seconds or so.  That means a decade's worth of downloads run at best 10*12 * 4 = 480 seconds ~ 8 minutes.  In practice we are often seeing longer delays, probably depending on the load on the NOAA servers.

Data was downloaded to five files:
longdailydata.csv
longmonthlydata.csv
widemonthlydata.csv
longannualdata.csv
wideannualdata.csv

##########################
####  Other Data Downloads
##########################
In searching for alternative data on which to base analysis of precipitation "events" over 24 hours, I found this:
https://hdsc.nws.noaa.gov/hdsc/pfds/pfds_map_cont.html?bkmrk=me
Which contains modeled return intervals, but no historical perspective.  The web page refers to Climate Data Online as its source.

Returning to CDO, I searched through the following link
https://www.ncei.noaa.gov/access/search/data-search/global-hourly
Which appears to offer hourly data back to 1940 dates for Portland jetport. I have not created python code to access these data,but that should be possible.

I submitted a request to CDO, requesting only precipitation data.  Original file downoaded March 9, 2019. by Curtis C. Bohlen. Data arrived as "2070949.csv", and was renamed "hourly_raw.csv."

This file is confusing in its layout, but it does link to the descriptions in the associated documentation.  it appears that when I selected "precipitation" data only, all I got were data on precipitation, but many different versions.  We need to use the various available codes to interpret the data by SOURCE and REPORT TYPE.  So far, we have not done so.
