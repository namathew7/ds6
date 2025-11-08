# TTC Streetcar Delay Analysis

### Data Science Intitute - Cohort 6 — Team 7 - Final Project
For our capstone project in the Data Sciences Certificate program at the University of Toronto’s Data Sciences Institute, we set out to explore real-world questions using a dataset on TTC streetcar delays.

### Members:
- Chloe Li
- Christine Romano
- Natasha Mathew
- Semiha Demirbas Caglayan
- Tanner Ferreira

## Project Overview
- Purpose and Overview
- Methodology
- Project Scope
- Understanding the Data
- Data Cleaning
- Data Analysis
- Conclusion
- Team Videos
- Credits and Source

## Purpose and Overview
Source: [City of Toronto’s Open Data Portal: TTC Streetcar Delay Data](https://open.toronto.ca/dataset/ttc-streetcar-delay-data/)

## Research Question
Can we use regression models to predict delay time based on other features in the dataset (line, date/time (month of year, seasons, holidays, week, day of month, day of week, time of day), direction, incident)?

The goal of this project is to analyze, visualize, and predict TTC streetcar minutes of delay with our regression model. Our team aims to uncover insights into delay trends across different features such as:
- Date/time (month of year, week of year, day of month, day of week, hour of day, seasons, holidays)
- Streetcar Line
- Streetcar Stop Location
- Incident
- Direction Bound
- Vehicle Number

This project applies regression modeling to understand the impact of date, time of day and ___ on minutes of delay. Our primary stakeholders include the operations team, planning team, and data team, as they are responsible for building predictive models and using the insights to optimize TTC streetcar schedules. This project is relevant to them because our model hopefully accurately predicts minutes of delay will help them allocate resources effectively, schedule streetcars efficiently, and improve long-term transit service. 

We don't know anything about how the data was collected, how data was sampled (target pop, frame pop, or sample pop) 

The data recording practice raises concerns about its representativeness of the overall customer base, as multiple rows were deleted due to incorrect spelling of streetcar stops.

By leveraging Python-based visualization libraries such as Matplotlib, Seaborn, and Plotly in a Jupyter Notebook environment and regression models, we want to see if different feeatures will affect minutes of delay.

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
