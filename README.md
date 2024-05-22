# Cyclistic-Case-Study-
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

### Data Cleaning

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
