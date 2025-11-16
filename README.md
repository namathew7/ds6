# Capstone Project - DSI Cohort 7 - DS Team 6

# Analyzing TTC Data on Streetcar Delays in 2024
For our capstone project in the Data Sciences Certificate program at the University of Toronto’s Data Sciences Institute, we set out to explore real-world questions using the 2024 dataset on Toronto Transit Commission's (TTC) streetcar delays.

![More reliable trips](Images/More_reliable_trips.png)

## Project Sections
Purpose and Overview \
Methodology \
Project Scope \
Understanding the Data \
Data Cleaning

## Purpose and Overview

The goal of this project is to analyze and visualize trends in TTC streetcar delays using the 2024 dataset. We will investigate if it is possible to correlate delay time with traits such as incident, bound direction, vehicle number, time of day, date, and streetcar line. The project aims to give TTC staff a better understanding of the factors that contribute to delay time, so that they can mitigate delays accordingly.

## Project Scope
This project involves conducting regression analysis on TTC streetcar delay data to estimate delay times based on key factors such as incident, bound direction, and time of day.
The resulting model could be valuable for:
Communicating approximate delay times to the public through various channels (e.g., apps, website, etc.).
Assisting in resource deployment by estimating the expected duration of delays (e.g., longer delays may require greater urgency and more staff).
The analysis will be validated through data splitting and performance metrics to ensure reliability. Additionally, the project will include insights and recommendations on the situations most likely to cause the longest delays. These insights could also inform HR planning by identifying patterns that impact staffing needs.

### Stakeholders
TTC Communications Team – Use insights to communicate accurate delay information.

TTC HR – Apply findings to staffing and hiring decisions.

Business Strategy & Insights Team – Leverage results to inform new route planning or network design.

TTC Users - Knowledge of factors likely to cause delays, which can help them plan their commutes. 

### Describing the Dataset

We used TTC streetcar delay data from Toronto’s open data catalogue for the 2024 year. While data for other years is available, we chose to use the 2024 dataset as it is a large sample (~14,000 rows) and it’s the most recent, complete year of data.

### Understanding the Features
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


## Methodology

### Data Cleaning Process

Upon preliminary inspection, several inconsistencies were observed in the “Location” column and there was wide variety in how the same locations were reported.  Examples include different versions of the same intersection (ex. “Yonge and Dundas” vs. “Dundas and Yonge”), misspellings (I can never spell “Roncesvalles” correctly, either), typos (“Bathust” vs. “Bathurst”), punctuation (“St. Clair” vs “St Clair”),  abbreviations (“DVP” vs. “Don Valley Parkway”), and different ways to describe the same street (ex. “Queen”, “Queen Street”, “Queen Street West”, “Queen St”, “Queen Street W”, etc.).  Given the diversity of these errors, it was deemed important to manually clean the original excel file.  Location names were processed through Excel’s spellcheck feature, and manually screened for issues with typos, punctuation, and naming inconsistencies.  This manual process reduced the number of unique locations from 2104 to 1573.  The excel file containing manually-cleaned location data is made available as  “ttc-streetcar-delay-data-2024_location_cleaning.xlsx”.  Given how variable the errors in “Location” were, it was very difficult to try and use a code-based cleaning method.  It is likely that the manual cleaning introduced irreproducibility, unintentional errors, and/or accidental misassignments.  To ensure quality location data in future years, we highly encourage the TTC to standardize the way they record location information.

After cleaning the “Location” data, the data were further cleaned in Excel to eliminate data not associated with one of the established TTC streetcar routes defined online: https://www.ttc.ca/routes-and-schedules/listroutes/streetcar.  Rows of data where the “Line” column did not match one of these lines were simply deleted.  For the “Bound” column, all data were supposed to be either blank or a cardinal direction (“N”, “S”, “E”, or “W”).  Entries not in one of these categories were removed from the dataset (7 rows). All columns were checked for null values, and it was found that only the “Bound” column contained null values. Since it was not possible to use imputation for this type of value, or remove them from the dataset entirely (1971 rows were null), we kept these rows as is. Finally, we checked that each column had the expected data type, and no errors were found.

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

### Enhancing the Data

As part of enhancing the data, several new columns were introduced: ‘Month’, ‘Week’, ‘Day of Month’, ‘Hour of Day’, and ‘Seasons’.  These columns are designed to further examine whether data on time of day or time of year and help predict length of delay or time of incident.  The resulting excel file is made available as “ttc-streetcar-delay-data-2024_cleaned.xlsx” and was used as the basis for all exploratory data analyses (shown here: https://drive.google.com/drive/u/0/folders/1Us1BW8OZXzJBEpC5erFqJYmCesQS-U9V).

#### Insights from Data Visualization

**Chart 1: Distribution of delay durations** 
*The most frequent duration of delays were found to be in the range of 11-15 minutes*

The vast majority of delay incidents fall within the shorter time ranges, with delays between 11–15 minutes being the most common at 5,408 incidents. This is followed by the 0–5 minute range with 2,868 incidents and the 6–10 minute range with 2,369 incidents, indicating that more than three-quarters of all delays occur within the first 15 minutes. After this point, the frequency of delays drops sharply: the 16–20 minute and 21–30 minute ranges show 682 and 977 incidents respectively, and the 31–45 minute range has 711 incidents. Longer delays are relatively rare, with only 301 incidents in the 46–60 minute range, 320 in the 61–90 minute range, 112 in the 91–120 minute range, and 182 incidents lasting 120 minutes or more. Overall, the data suggests that while long delays do occur, they make up a small portion of total incidents, and efforts to improve performance would be most impactful if focused on reducing the short but highly frequent delays—particularly those in the 11–15 minute range, where the highest concentration of issues is observed.

![Binned_Delay_Min](Images/Binned_Delay_Min.png)

**Chart 2: Distribution of Minute Delay by Incident Type (Box Plot)** 

Most incident types—such as cleaning, unsanitary conditions, mechanical issues, and security-related events—tend to produce shorter and more consistent delays, with relatively low medians and tight interquartile ranges. In contrast, several types display significantly higher variability and longer potential delays. Incidents related to operations stand out with the widest spread and the highest upper range, indicating that they can lead to severe delays far exceeding those of other categories. Collisions involving the TTC, emergency services, and overhead issues also show larger dispersions, suggesting a greater likelihood of prolonged disruptions. Meanwhile, categories such as diversions, held-by incidents, and rail/switch issues produce moderate delays with some variability but fewer extreme outliers. While many everyday incidents result in predictable, shorter delays (e.g., investigations, mechanical, operations, etc.), specific event types—particularly overhead, diversion, and collisions—are responsible for the longest and most unpredictable service interruptions.

![Min_Delay_by_Incident_Type](Images/Min_Delay_by_Incident_Type.png)

**Chart 3: Typical Delay Lengths per Incident** 

Diversions and overhead incidents were found to have the greatest average delay time per incident. However, it is important to note that there is a much smaller sample for overhead incidents.

Most incident types fall within a moderate delay range—generally between 8 and 15 minutes—including cleaning/unsanitary issues, diversion, general delay, mechanical incidents, investigation, operational events, security, and “utilized off route” cases. A few categories, however, stand out for causing substantially higher average delays. Emergency services incidents produce an average delay of about 23 minutes, while collisions involving the TTC create even longer delays at roughly 44 minutes. The most significant contributor to extended service disruptions is overhead-related incidents, with the highest average delay at approximately 50 minutes. These values indicate that while many everyday incident types result in relatively short delays, a small number of high-severity categories—particularly overhead issues, collisions, and emergency services—drive the longest average disruptions and have the greatest operational impact.

![Avg_Min_Delay_Time_By_Incident](Images/Avg_Min_Delay_Time_By_Incident.png)

**Charts 4 and 5: Temporal Patterns** 

Temporal patterns are important to understand as they can indicate important variables (hour, day, week, month, season). When looking at the time series data, there were no obvious monthly trends. Total minutes delayed by season was higher in the summer and winter than the fall and spring. In the summer, this is largely due to operations delays and in the winter, these delays could be due to weather. Fall was the season with the lowest frequency of delays. Operations delays peaked on Saturdays, which is often when the TTC has service disruptions. In the hourly data, there were peaks during the morning and evening rush hours, as expected. 

![minutes delayed by date](Images/minutes delayed by date.png)
![day-of-week_vs_delays](Images/day-of-week_vs_delays.png)

#### Spatial and Operational Insights 

There are 18 streetcar lines operating across Toronto, and it is important to explore their vulnerability to service disruptions. To accomplish this, additional data analyses were conducted to understand which lines experience the most severe operational challenges.

**Chart 6a: Lines With the Highest Average Delays** 

Based on the figure, Lines 303, 508, 310, 301, 305, 507, and 503 experience the longest delays per incident. Among these, Line 303 stands out with an average delay of approximately 48 minutes, followed by Line 508 at around 42 minutes. Lines 310, 301, and 305 also show significantly high average delays. These results indicate that although some of these lines may not encounter disruptions frequently, the delays that do occur tend to be long-lasting and operationally costly, likely due to route-specific constraints or limited opportunities for bypassing blockages.

**Chart 6b: Lines With the Most Delay Incidents** 

When examining the number of delay incidents, Lines 501, 504, 505, 510, 506, and 512 stand out as the most incident-prone routes. Line 501 records the highest number of delays with roughly 2,776 incidents, followed by 504 and 505 with 2,198 and 1,825 incidents, respectively. These lines typically serve dense urban corridors and experience high ridership and traffic interaction, contributing to more frequent operational disruptions. Their vulnerability stems not from delay duration but from the sheer frequency of incidents requiring intervention.

**Chart 6c: Lines With the Most Unpredictable Delays** 

The variability in delay duration reveals another dimension of vulnerability. Lines 508, 303, 310, 511, and 301 exhibit the largest fluctuations, with Line 508 showing an exceptionally high standard deviation of nearly 109 minutes. This unpredictability suggests that delays on these lines are difficult to manage because disruptions can escalate quickly and inconsistently. Such variability is often associated with routes that share significant portions of track with regular traffic, have fewer passing opportunities, or operate through segments prone to sudden congestion or blockages.

![Line_vulnerability_Combo](Images/Line_vulnerability_Combo.png)

**Chart 7: Severity of Incidents**  

Another important attribute of delay is understanding the pattern of incident types and how they vary in severity. The stacked bar chart illustrates how each incident category contributes to minor, moderate, and major delays across the TTC streetcar network. Severity is classified into three levels: Minor delays lasting less than 10 minutes, Moderate delays lasting 10–25 minutes, and Major delays exceeding 25 minutes.

This visualization highlights that certain incident types such as “Operations, Mechanical, Diversion, and Emergency Services” not only occur frequently but also generate a significant share of moderate and major delays. In contrast, categories like “Rail/Switches, Utilized Off Route, and Overhead” tend to produce mostly minor delays, indicating they are typically resolved more quickly. The distribution of severity across categories provides valuable insight into which incident types have the greatest operational impact, helping identify where targeted mitigation strategies may reduce overall service disruption.

![incidient severity distribution type](Images/incidient severity distribution type.png)

**Chart 8: Correlation Analysis** 

Before progressing to modelling, a correlation heatmap is generated to explore the relationships among the key attributes, including both numerical variables and encoded categorical fields. This visualization helps identify whether any strong linear associations exist that could influence model performance, introduce redundancy, or highlight meaningful patterns in the delay data.

Overall, the heatmap shows very weak correlations among most variables, indicating that no single attribute strongly predicts delay duration or gap time on its own. The only prominent relationship is the expected high correlation between Min Delay and Min Gap, as both metrics reflect components of the overall disruption duration. A moderate correlation is also visible between Line and Time of Day, suggesting that certain lines operate more frequently or more heavily during specific hours, but this pattern is not strong enough to dominate model behaviour. Seasonal, daily, and date-related fields show very low correlations with operational attributes such as delays, incidents, or vehicle number, reinforcing the idea that delays are influenced by complex, non-linear, and multi-factor interactions rather than simple linear relationships.

![Correlation_heat_map1](Images/Correlation_heat_map1.png)

**Chart 9: Temporal Vulnerability Patterns** 

The temporal vulnerability heatmap illustrates how average delay varies across streetcar lines during different times of day. This visualization helps identify when specific lines are most prone to disruption and reveals important time-dependent operational patterns across the network.

A striking observation is that most streetcar lines maintain relatively consistent delay levels across time periods, with moderate increases during high-demand windows such as morning (9–12) and evening (18–21). However, several lines show distinct temporal peaks. Line 508, in particular, exhibits an extreme delay spike during the midday period (12–15), reaching an average delay of approximately 126 minutes, making it the most temporally vulnerable route in the dataset. Lines 303, 310, and 511 also show notable increases during specific periods such as early morning and late night, suggesting sensitivity to operational factors such as reduced service frequency, construction activity, or traffic interactions.

Additionally, heavily used lines like 501, 503, 504, 506, 507, and 512 show elevated midday and afternoon delays, which align with typical traffic congestion and higher passenger volumes during these hours. The pattern indicates that both operational constraints and city-wide traffic dynamics contribute to higher delay durations during middle-of-day periods for several key routes.

![heat2](Images/heat2.png)

**Chart 10a and 10b: Delays By Location** 

We were particularly interested in learning whether certain locations were associated with increased delays.  Of the 1400+ unique locations in our cleaned database, only 27 were responsible for more than 100 delays in 2024.  That means, fewer than 2% of the locations hosted nearly a third of delay incidents and a quarter of total delay time.  The TTC could make a large impact with few resources by focusing on these locations.

In the bar chart, the x axis shows high incident locations.  The left y axis shows the number of delay incidents per year, color coded by incident type.  Broadview and Spadina stations seem to have numerous sanitation and emergency services delays, while Kingston Loop and Kingston Road Loop  had an alarming number of security delays.  The TTC could minimize delays by coordinating with cleaning and emergency crews at Broadview and Dundas West stations to ensure better access and / or more frequent servicing.  To address the security issues at Kingston Loop and Kingston Road Loop, the TTC may want to partner with local law enforcement.

Union Station remains plagued by a high relative percentage of general delays, but this “insight” will not surprise any Torontonian.

Data on accumulated delay time are overlaid as a line plot.  The location with the high total delay time in 2024 was King and Church, and I suspect the long delays were the result of diversions.

![Location_Incident_Percentages](Images/Location_Incident_Percentages.png)
![Images/Streetcar_Delays_Bar_Chart_2024](Images/Streetcar_Delays_Bar_Chart_2024.png)

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
