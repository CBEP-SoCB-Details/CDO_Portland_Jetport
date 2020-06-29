# Portland Jetport Climate Data Analysis
<img
    src="https://www.cascobayestuary.org/wp-content/uploads/2014/04/logo_sm.jpg"
    style="position:absolute;top:10px;right:50px;" />

Data analysis archive for analyzing the eighty year history of weather observations at the Portland Jetport.

# Statement of Purpose
CBEP is committed to the ideal of open science.  Our State of the Bay data archives ensure the science underlying the 2020 State of the Bay report is documented and reproducible by others. The purpose of these archives is to release raw data and data analysis code whenever possible to allow others to review, critique, learn from, and build upon CBEP science.

# Archive Organization
All CBEP 2020 State of the Bay data analysis repositories are divided into four sub-folders.  Each subfolder contains data, code and analysis results as follows:  

- Original Data.  Original data, with a "READ ME.TXT" file that documents data sources.  DATA IN THIS FOLDER IS AS ORIGINALLY PROVIDED OR ACCESSED.
- Derived Data.  Data derived from the originaL RAW DATA.  Includes documentation of data reorganization steps, either in the form of files (R notebooks, Excel files, etc.) that embody data transformations, or via another README.txt file
- Analysis.  Contains one or more R Notebooks proceeding through the data analysis steps.
- Graphics.  Contains R Notebooks stepping through development of related graphics, and also raw copies of resulting graphics, usually in \*.png and \*.pdf formats.  These graphics may differ from graphics as they appear in final State of the Bay graphical layouts.

# Summary of Data Sources
All data are derived from data available from NOAA National Centers for Environmental Information (NOAA NCEI).  Raw data were downloaded from NOAA's online repositories using a Python script using a Public NOAA API.  Details are included in a "READ ME.txt"" file in the "Original Data" folder.

We principally used two different data sets, both accessed through the API.
- GHCND.  Global Historical Climatology Network Daily; Daily data; used here to derive length of the growing season, and number of very large storm days (> 2 inches of rain) each year.
- GSOY (Global Summary of the Year; Yearly Summaries; used here for all other metrics).

# Links to R Notebooks
## Data Analysis
[Growing Season](./Analysis/Analysis_of_length_of_growing_season_3.Rmd)  
[Number of high and low temperatures and high rainfall days](Frequency_Analysis_2.Rmd)  
[Annual Temperature Extreams](Analysis_of_max_min_and_averages.Rmd)  

## Graphics
[Growing Season](length_of_growing_season_3.Rmd)  
[Frequency of Extreme Events](Frequency_Analysis_Graphics.Rmd)  
[Frequency of Extreme Events, Revised](Frequency-Analysis-Single-Graphics.Rmd)  
[Annual Temperature Extreams](max_min_and_averages.Rmd)  
[Annual Temperature Extreams, Revised](max_min_and_averages_single_graphics.Rmd)  

