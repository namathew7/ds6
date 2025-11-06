# Capstone Project - DSI Cohort 7 - DS Team 6
## Research Question & Business Problem
### Regression: Can we predict delay time based on the other variables (line, date(month, seasons, holidays, week, day of month)), day of week, time(hour), direction, incident)?

## Industry Context

## Describing the Dataset

## Data Cleaning Process

### Visualizations:
  1. Pick a streetcar route - look at type of incidents and map it by frequency (SC)
  2. Location-based incidents across dataset - filter for n > 5 (CR)
  3. Panel plot of incident types by date (CL)

### Cleaning the Map Dataset:
  1. Map longtitude & latitude to stop names for one streetcar route (SC) - 501 or 504

### Cleaning the Main Dataset (work off CR's cleaned dataset): (NM)
  1. Remove rows with invalid (not known streetcar line) data
  2. Remove rows without a streetcar route
  3. Create extra columns for: month, season, day of month, week - extract from date
  4. Create extra column for: hour of day - extract from time

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
