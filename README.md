# Cyclistic-Case-Study

## Defining the Problem

The core challenge facing the director of marketing and the marketing analytics team is to develop effective marketing strategies that facilitate the conversion of Cyclistic's casual riders into committed annual members. This analysis is guided by three essential questions that will shape the upcoming marketing program:

1. **Differential Usage Patterns**: How do annual members and casual riders use Cyclistic bikes differently?
2. **Motivation for Membership**: Why would casual riders buy Cyclistic annual memberships?
3. **Utilize Digital Media**: How can Cyclistic use digital media to influence casual riders to become members?

In this project, the scope will only include the differential usage patterns of annual members and casual riders when using Cyclistic bikes.

Analyzing the data allows us to find major patterns in different customer groups. This study will create exact profiles for each type of customer, helping the marketing team make specific plans to turn casual riders into committed members. These insights have two goals: helping Cyclistic's leaders increase annual memberships for future growth, and giving a strong base for improved, personalized marketing strategies.

## Scenario

This case study centers on Cyclistic's evolution from a successful bike-share launch in 2016 to its present status as a major player in Chicago's urban mobility landscape. With 5,824 bicycles stationed at 692 locations, Cyclistic's flexible pricing plans have attracted both casual riders and annual members. However, a clear financial distinction has emerged, prompting a strategic shift towards maximizing annual memberships for future growth.

The marketing analytics team's objective is to understand the distinctive usage patterns of Cyclistic bikes between casual riders and annual members. These insights will serve as the foundation for an innovative marketing strategy to convert casual riders into committed annual members. The main beneficiaries of this project include Cyclistic’s director of marketing and the executive team. Meanwhile, the Cyclistic marketing analytics team assumes a secondary stakeholder role in this analysis.

## Business Task

Analyze the distinctive ways in which annual members and casual riders utilize Cyclistic bikes, focusing on the differential usage patterns.

## Data Sources

The data for this analysis is Cyclistic's historical trip data, sourced from the previous 12 months. This dataset is publicly available [here](#link-to-dataset) and has been provided under a license by Motivate International Inc. The primary focus of this analysis is to explore usage trends among different customer segments without compromising into personally identifiable information, ensuring compliance with data privacy regulations.

There are 12 .CSV files and the dataset includes crucial details about:
- Bike trips data, such as their duration, starting and ending stations, and whether the rider is a member or casual user.

**File Names**:
```sql
1. da-project-cyclistic.DA_Cyclistic.2022_08
2. da-project-cyclistic.DA_Cyclistic.2022_09
3. da-project-cyclistic.DA_Cyclistic.2022_10
4. da-project-cyclistic.DA_Cyclistic.2022_11
5. da-project-cyclistic.DA_Cyclistic.2022_12
6. da-project-cyclistic.DA_Cyclistic.2023_01
7. da-project-cyclistic.DA_Cyclistic.2023_02
8. da-project-cyclistic.DA_Cyclistic.2023_03
9. da-project-cyclistic.DA_Cyclistic.2023_04
10. da-project-cyclistic.DA_Cyclistic.2023_05
11. da-project-cyclistic.DA_Cyclistic.2023_06
12. da-project-cyclistic.DA_Cyclistic.2023_07
```

**Columns**:
```sql
- ride_id
- rideable_type
- started_at
- ended_at
- start_station_name
- start_station_id
- end_station_name
- end_station_id
- start_lat
- start_lng
- end_lat
- end_lng
- member_casual
```
A significant concern revolves around ensuring the data fairly represents all user groups and is comprehensive. Our evaluation aligns with the ROCCC principle (Reliability, Originality, Comprehensiveness, Currentness, Context) to validate the data's credibility.

The dataset's importance lies in its potential to reveal how Cyclistic's riders behave. It helps identify trends in trips and rider preferences. Nonetheless, a cautious analysis is essential due to potential flaws and biases that might affect how we interpret the data.

Thorough data verification protocols are established, involving checks for consistency and identification of anomalies. This ensures the dataset's accuracy and reliability.

## Data Cleaning & Manipulation

In this step, we ensured that the data is stored in an appropriate manner and is ready for analysis. To initiate this process, the downloaded zip files are extracted to reveal the contained information. Subsequently, a dedicated folder is created, within this main folder, organization is further enhanced by establishing two distinct subfolders. One subfolder is designated for .CSV files, while the other accommodates .XLS or Sheets files. This division ensures the preservation of the original data sets.

### Microsoft Excel Spreadsheets, Data Manipulation:

For Excel, the process starts by launching the application, opening each file, and opting to "Save As" an Excel Workbook file. These newly saved files find their place within the subfolder dedicated to .XLS files.

1. **Created a column called ride_length:**
   - Calculated the length of each ride by subtracting the ended_at column from the started_at column.
   - Formula: `= D2-C2` (when `ended_at = D2` and `started_at = C2`)
   - Formatted as `TIME`: Format → Cells → Time → `HH:MM:SS`

2. **Created a column called weekday:**
   - Obtained the weekday that each ride started using the WEEKDAY command.
   - Formula: `= WEEKDAY(C2,1))`
   - Formatted as an `INTEGER`: Format → Cells → Number (When `1 = Sunday`)

After this step, I utilized PivotTables to further analyze the data.

1. **Created a PivotTable:**
   - Insert → PivotTable
   - Selected a table or range
   - Chose where to place the PivotTable

I repeated this process for each .XLS file to create a first insight into the data behavior such as the total number of rides per month, total number of rides per member or casual rider, average ride length, and identify the most prominent days of the week for Cyclistic's riders.

To finalize this process, I saved all the updates in an .XLS file and made a copy on a .CSV file.

## Exploratory Data Analysis

### BigQuery

To facilitate the analysis, I opted to employ SQL via BigQuery for its collective Google Cloud Storage that makes the transition to manage extensive data volumes easy.

## Data Cleaning

#### Yearly Data

To consolidate the data for easier cleaning and manipulation, I created a new table within the same dataset by combining all the monthly files into a new table called `yearly_data`.

```sql
-- COMBINING ALL TABLES
CREATE TABLE da-project-cyclistic.DA_Cyclistic.yearly_data AS
SELECT * FROM (
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2022_08
UNION ALL
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2022_09
UNION ALL
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2022_10
UNION ALL
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2022_11
UNION ALL
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2022_12
UNION ALL
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2023_01
UNION ALL
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2023_02
UNION ALL
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2023_03
UNION ALL
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2023_04
UNION ALL
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2023_05
UNION ALL
SELECT * FROM da-project-cyclistic.DA_Cyclistic.2023_07
);
```

After ensuring that there are no duplicates to avoid inaccurate analysis and reporting, I proceeded with my analysis.

### Total Rides

I calculated the total trips to get a clear picture of how trips are distributed between different user types (members and casual riders).

```sql
-- CALCULATE THE TOTAL TRIPS AND WHAT PERCENTAGE EACH USER_TYPE REPRESENTS --
SELECT 
    user_type, 
    COUNT(*) AS Total_trips,
    COUNT(*) * 100.0 / SUM(COUNT(*)) OVER () AS Trips_Percentage
FROM 
    `da-project-cyclistic.DA_Cyclistic.DATA2022_2023` 
GROUP BY 
    user_type;
```

It helped me to identify which user type contributes more to the total number of trips and by how much. The percentage column allowed me to compare the engagement of each user type relative to the total number of trips.

### Average Ride Length 

This query provides the average ride time (in total minutes) for each user type in the specified dataset.

```sql
-- CALCULATE THE RIDE LENGTH PER DAY FOR CASUAL RIDERS --
SELECT 
    user_type,
    EXTRACT(DAYOFWEEK FROM started_at) AS day_of_week,
    AVG(Total_minutes) AS avg_ride_length_minutes
FROM `DA_Cyclistic.DATA2022_2023`
WHERE user_type ='casual'
GROUP BY user_type, day_of_week
ORDER BY user_type, day_of_week;
```

```sql
-- CALCULATE THE RIDE LENGTH PER DAY FOR MEMBER RIDERS --
SELECT 
    user_type,
    EXTRACT(DAYOFWEEK FROM started_at) AS day_of_week,
    AVG(Total_minutes) AS avg_ride_length_minutes
FROM `DA_Cyclistic.DATA2022_2023`
WHERE user_type ='member'
GROUP BY user_type, day_of_week
ORDER BY user_type, day_of_week;
```

I calculated the average ride length in minutes per day of the week for each user type. We can see that casual users spend more time cycling than annual members, taking longer rides on days like Saturdays and Sundays.

## Total Rides Per Day Of The Week

I calculated a breakdown of the total number of rides and the percentage of rides for each day of the week in the specified dataset. This analysis is valuable to understand the distribution of rides throughout the week and identifying patterns in rider behavior based on different days.

```sql
-- CALCULATE THE RIDES PER DAY OF THE WEEK --
SELECT 
    EXTRACT(DAYOFWEEK FROM started_at) AS day_of_week,
    COUNT(*) AS total_rides,
    COUNT(*) * 100.0 / SUM(COUNT(*)) OVER () AS percentage
FROM `DA_Cyclistic.DATA2022_2023`
GROUP BY day_of_week
ORDER BY day_of_week;
```
## Rides Per Season

In summary, this query provides a breakdown of the total number of rides per season in the specified dataset. The results will show the different seasons and the total number of rides for each season. This analysis could be valuable for understanding seasonal patterns and trends in ride usage.

```sql
-- CALCULATE THE RIDES PER SEASON --
SELECT
    CASE
        WHEN EXTRACT(MONTH FROM started_at) IN (3, 4, 5) THEN 'Spring'
        WHEN EXTRACT(MONTH FROM started_at) IN (6, 7, 8) THEN 'Summer'
        WHEN EXTRACT(MONTH FROM started_at) IN (9, 10, 11) THEN 'Fall'
        ELSE 'Winter'
    END AS season,
    user_type,
    COUNT(*) AS total_rides
FROM `DA_Cyclistic.DATA2022_2023`
GROUP BY season, user_type
ORDER BY user_type, season;
```
## Rides Per Time Periods

This query provided a breakdown of the total number of rides per time periods (morning, afternoon, night) for all user types.

```sql
-- GROUP BY THE TIME PERIODS --
SELECT
    CASE
        WHEN EXTRACT(HOUR FROM started_at) >= 5 AND EXTRACT(HOUR FROM started_at) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM started_at) >= 12 AND EXTRACT(HOUR FROM started_at) < 18 THEN 'Afternoon'
        ELSE 'Night'
    END AS time_of_day,
    user_type,
    COUNT(*) AS total_rides
FROM `DA_Cyclistic.DATA2022_2023`
GROUP BY time_of_day, user_type
ORDER BY user_type, time_of_day;
```
The busiest time during the day to get a bike is during the afternoon. We can see that both casual and member users tend to ride during the afternoon, but differ on the morning period where casual users drop significantly.

## Frequently Used Stations

I calculated the most frequent stations based on ride counts in the specified dataset. The results will show the station names and their corresponding ride counts, ordered from the highest ride count to the lowest.

```sql
-- CALCULATE THE MOST FRECUENT STATIONS --
SELECT 
    station_name, 
    COUNT(*) AS ride_count 
FROM (
    SELECT start_station_name_clean AS station_name FROM da-project-cyclistic.DA_Cyclistic.DATA2022_2023
    UNION ALL
    SELECT end_station_name_cleaned AS station_name FROM da-project-cyclistic.DA_Cyclistic.DATA2022_2023
) AS all_stations
GROUP BY station_name
ORDER BY ride_count DESC;
```

This analysis provides insights into the popularity and usage of different stations within the cycling dataset. We can see that the stations close to the harbor are the most popular such as Streeter Dr & Grand Ave and DuSable Lake Shore. An initial hypothesis could be that most users bike during the weekends close to tourist attractions; therefore, we will need to target those areas to convert casual riders using this program as an entertainment to annual members.

## Conclusion

In this section, we analyze the distinctive ways in which annual members and casual riders utilize Cyclistic bikes, focusing on the differential usage patterns. The analysis is presented in a reader-friendly, humanized format, highlighting key insights and recommendations.

### Seasonal Trends: Spring - Summer as Peak Seasons

During the peak seasons of spring and summer, Cyclistic can strategically amplify its marketing efforts to establish a strong foundation of committed annual members. Two key strategies are proposed:

- **Introduce limited-time seasonal membership offers tailored to the peak seasons and provide exclusive discounts or extended membership benefits for users who commit during these periods.** This approach incentivizes potential customers to sign up for annual memberships during the peak cycling seasons.

- **Distribute an educational newsletter to emphasize the joy of cycling in pleasant weather and showcase how being an annual member guarantees access to enjoyable rides anytime.** This initiative aims to educate and inspire potential members about the advantages of annual memberships, particularly during the peak cycling months.

### Mobility Motives: Commuting vs. Entertainment

The data reveals that a significant portion of annual members use Cyclistic bikes for commuting purposes, while casual users tend to ride for entertainment purposes. This distinction presents a valuable opportunity to cater to their distinct needs through targeted marketing strategies:

- **Develop marketing campaigns that position Cyclistic as the preferred mobility solution for event attendees.** Highlight the convenience of bike-sharing during crowded events, reducing traffic congestion and offering a unique urban experience. This strategy targets casual users who ride for entertainment purposes.

- **Introduce tailored incentives, such as discounted morning rides or promotional campaigns, to persuade casual users to consider biking as a viable commuting option.** This approach aims to convert casual users into potential annual members by highlighting the benefits of bike commuting.

### Localized Outreach

To further enhance Cyclistic's visibility and appeal, consider the following localized outreach strategies:

- **Strategically position Cyclistic stations near popular event venues or tourist attractions to capture the attention of potential casual users.**

- **Utilize geo-targeted advertisements to raise awareness among event-goers and tourists.** Consider collaborating with event organizers to offer promotional codes or incentives to attendees, encouraging them to become annual members.

- **Develop a feature within Cyclistic's app that suggests scenic routes to popular tourist attractions.** Emphasize the enjoyable experience of cycling to entertainment venues while saving on parking fees and avoiding traffic congestion. This strategy caters to the needs of casual users who ride for entertainment purposes.

By implementing these targeted strategies, Cyclistic can effectively address the distinct needs of annual members and casual riders, leveraging their respective usage patterns to drive membership growth and customer loyalty.
