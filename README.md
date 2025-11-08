# Capstone Project - DSI Cohort 7 - DS Team 6

### Project Sections
Purpose and Overview
Methodology
Project Scope
Understanding the Data
Data Cleaning

### Purpose and Overview

The goal of this project is to analyze and visualize trends in TTC streetcar delays using the 2024 dataset. We will investigate if it’s possible to predict delay time by using predictors such as incident, bound direction, vehicle number, time of day, date, and streetcar line. The project aims to give TTC staff a better understanding of the factors that contribute to delay time, so that they can mitigate delays accordingly.

### Project Scope
This project involves conducting regression analysis on TTC streetcar delay data to estimate delay times based on key factors such as incident, bound direction, and time of day.
The resulting model could be valuable for:
Communicating approximate delay times to the public through various channels (e.g., apps, website, etc.).
Assisting in resource deployment by estimating the expected duration of delays (e.g., longer delays may require greater urgency and more staff).
The analysis will be validated through data splitting and performance metrics to ensure reliability. Additionally, the project will include insights and recommendations on the situations most likely to cause the longest delays. These insights could also inform HR planning by identifying patterns that impact staffing needs.

## Stakeholders
TTC Communications Team – Use insights to communicate accurate delay information.

TTC HR – Apply findings to staffing and hiring decisions.

Business Strategy & Insights Team – Leverage results to inform new route planning or network design.

## Describing the Dataset

We used TTC streetcar delay data from Toronto’s open data catalogue for the 2024 year. While data for other years is available, we chose to use the 2024 dataset as it is a large sample (~14,000 rows) and it’s the most recent, complete year of data.

## Understanding the Features
| Column Name | Description                                      |
|-------------|--------------------------------------------------|
| Date        | Incident date (MM/DD/YYYY)                       |
| Line        | Streetcar route number                           |
| Time        | Time of day (24h clock)                          |
| Day         | Day of the week                                  |
| Location    | Location of incident (intersection/ street)      |
| Incident    | Type of incident                                 |
| Min Delay   | Minutes of delay to streetcar service            |
| Min Gap     | Minutes between streetcars                       |
| Bound       | The direction of the streetcar travel            |
| Vehicle     | Streetcar vehicle number                         |


### Methodology

## Data Cleaning Process

Upon preliminary inspection, several inconsistencies were observed in the “Location” column and there was wide variety in how the same locations were reported.  Examples include different versions of the same intersection (ex. “Yonge and Dundas” vs. “Dundas and Yonge”), misspellings (I can never spell “Roncesvalles” correctly, either), typos (“Bathust” vs. “Bathurst”), punctuation (“St. Clair” vs “St Clair”),  abbreviations (“DVP” vs. “Don Valley Parkway”), and different ways to describe the same street (ex. “Queen”, “Queen Street”, “Queen Street West”, “Queen St”, “Queen Street W”, etc.).  Given the diversity of these errors, it was deemed important to manually clean the original excel file.  Location names were processed through Excel’s spellcheck feature, and manually screened for issues with typos, punctuation, and naming inconsistencies.  This manual process reduced the number of unique locations from 21043 to 15732.  The excel file containing manually-cleaned location data is made available as  “ttc-streetcar-delay-data-2024_location_cleaning.xlsx”.  Given how variable the errors in “Location” were, it was very difficult to try and use a code-based cleaning method.  It is likely that the manual cleaning introduced irreproducibility, unintentional errors, and/or accidental misassignments.  To ensure quality location data in future years, we highly encourage the TTC to standardize the way they record location information.

AAfter cleaning the “Location” data, the data were further cleaned in eExcel to eliminate data not associated with one of the established TTC streetcar routes defined online: https://www.ttc.ca/routes-and-schedules/listroutes/streetcar.  Rows of data where the “Line” column did not match one of these lines were simply deleted.  For the “Bound” column, all data were supposed to be either blank or a cardinal direction (“N”, “S”, “E”, or “W”).  Entries not in one of these categories were removed from the dataset (7 rows). All columns were checked for null values, and it was found that only the “Bound” column contained null values. Since it was not possible to use imputation for this type of value, or remove them from the dataset entirely (1971 rows were null), we kept these rows as is. Finally, we checked that each column had the expected data type, and no errors were found.

The following table describes the unique values in each column, before and after cleaning:

| Column Name | Unique Values Before Cleaning | Unique Values After Cleaning |
|-------------|-------------------------------|------------------------------|
| Date        | 366                           | 366                          |
| Line        | 42                            | 18                           |
| Time        | 1438                          | 1438                         |
| Day         | 7                             | 7                            |
| Location    | 2104                          | 1573                         |
| Incident    | 13                            | 13                           |
| Min Delay   | 228                           | 227                          |
| Min Gap     | 239                           | 238                          |
| Bound       | 7                             | 5                            |
| Vehicle     | 1176                          | 1151                         |

## Enhancing the Data

As part of enhancing the data, several new columns were introduced: ‘Month’, ‘Week’, ‘Day of Month’, ‘Hour of Day’, and ‘Seasons’.  These columns are designed to further examine whether data on time of day or time of year and help predict length of delay or time of incident.  The resulting excel file is made available as “ttc-streetcar-delay-data-2024_cleaned.xlsx” and was used as the basis for all exploratory data analyses (shown here: https://drive.google.com/drive/u/0/folders/1Us1BW8OZXzJBEpC5erFqJYmCesQS-U9V).

-----------
## Brainstorming Notes Below
### Visualizations:
  1. Pick a streetcar route - look at type of incidents and map it by frequency (SC)
  2. Location-based incidents across dataset - filter for n > 5 (CR)
  3. Panel plot of incident types by date (CL)

### Cleaning the Map Dataset:
  1. Map longtitude & latitude to stop names for one streetcar route (SC) - 501 or 504

## Exploratory Analysis:
  1. Pairwise plots:
     a. Min delay vs incident (CR)
     b. Min delay vs route (TF)
     c. Min delay vs direction (CR)
     d. Min delay vs min gap (TF)
  2. Histograms:
     a. Frequency of min delays by duration (SC)
     b. Frequency of incidents (SC)
  3. Time series graph of frequency of total incidents by date (CL)
  4. Time series graph of total minute delay by date (NM)

## Ideas for Later:
  1. Categorize incidents into larger groups
