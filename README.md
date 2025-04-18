# **BELLABEAT ANALYSIS CASE STUDY** 

Dr. Irtwange Nguwasen Blessing

2025-03-15

## SCENARIO 

![Bellabeat](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/Bellabeat.png?raw=true)


As a junior data analyst who joined the Bellabeat marketing analytics team 6 months ago, a high-tech manufacturer of health-focused products for women.I am tasked to analyze smart device usage data for a Bellabeat product to uncover trends in consumer behavior. The goal is to identify how people use non-Bellabeat smart devices and apply these insights to one of Bellabeat’s products. My findings will help shape Bellabeat’s marketing strategy and support its growth in the global smart device market.


## BACKGROUND:
Bellabeat was founded in 2013 by Urška Sršen and Sando Mur. It's a high-tech company that manufactures health-focused smart products for women, collecting data on activity, sleep, stress, and reproductive health. It aims to empower women with
knowledge about their own health and habits.
By 2016, Bellabeat had opened offices around the world and launched multiple products such as :


1) Bellabeat app - provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits.


2) Leaf: a wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf tracker connects to the Bellabeat app to track activity, sleep, and stress.


3) Time: a wellness watch combines the timeless look of a classic timepiece with smart technology to track user activity, sleep, and stress.


4) Spring: a water bottle that tracks daily water intake using smart technology to ensure that you are appropriately hydrated throughout the day.


5) Bellabeat membership: a subscription-based membership program for users. Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health and beauty, and mindfulness based on their lifestyle and goals.


### ASK PHASE


#### Business Objectives
1) Identify trends in smart device usage.
2) Apply these trends to Bellabeat’s target customers.
3) Use the insights to guide Bellabeat’s marketing strategy.


#### Key stakeholders:
1) Urška Sršen – Co-founder & Chief Creative Officer (CCO)
2) Sando Mur – Co-founder & Mathematician
3) Bellabeat Marketing Analytics Team
4) Bellabeat Executive Team
5) Bellabeat Customers


### PREPARE PHASE


*	Extracted and worked with data from [FitBit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit) (CC0: Public Domain, dataset made available through [Mobius](https://www.kaggle.com/arashnic)).
*	Description of all data sources used
1.	Data location; public data, made available on Kaggle by Mobius.
2.	Data organization; 
a)	There's bias, data may be credible. 
b)	ROCCC- Data is non-reliable, original, comprehensive, historic and cited.
c)	Data is licensed, secure, private and accessible to only me.

#### Limitations
1) Small sample size of 30 Fitbit users which is not a representative of a broader population
2) Unknown Fitbit user demographic, age, health status or lifestyle (Bellabeat’s target audience (women)).
3) Data collection bias since the users voluntarily provided their Fitbit data, This means self-selection bias is present.
4) Data is from 2016, which is outdated and may not be relevant to current trends.
5) Data is from Fitbit users, not Bellabeat users, so the analysis may not be applicable to Bellabeat users,as they may have different usuage patterns.
6) Device-Specific Limitations - The data collected from Fitbit devices may have a different tracking algorithms than the Bellabeat devices, which may also affect the analysis.


#### Choice of the Most Relevant Product
Bellabeat has multiple products, but I need to select one based on available data. Since the dataset contains activity, sleep, heart rate, and weight data, the most relevant Bellabeat products are:


Leaf (Wearable wellness tracker) – Tracks activity, sleep, and stress.


Time (Smart wellness watch) – Tracks activity, sleep, and stress.


Since both products track similar metrics, I would focus on Leaf, as it aligns closely with Fitbit-style smart device usage.


#### Justification for Data Choice
The Fitbit dataset contains different types of data at different time intervals. The most useful datasets for analyzing user trends relevant to **Leaf** are:

1) Activity Data (Daily & Hourly) - To understand how active users are throughout the day, peak activity times, and how movement patterns influence overall wellness.
2) Sleep Data - To analyze sleep trends, identify common sleep duration patterns, and evaluate how they relate to activity levels.
3)  Heart Rate Data - To explore stress and cardiovascular activity trends that could relate to Leaf’s stress-monitoring feature.Because heartrate_seconds_merged data is too detailed, I will use daily activity metrics to get an overview of activity intensity and stress levels.
4) Weight log data - To understand how weight changes over time and how it relates to activity levels and sleep pattern. This will not be used as weight data is often self-reported and not consistent across users.


### PROCESSING PHASE


Tools to used are : 
a)	R- To manipulate, clean and analyze data.
b)	Tableau- For some data visualization


#### Data manipulation and cleaning.


I. Data Manipulation


1. load relevant libraries
```{r load packages, message=FALSE, warning=FALSE, paged.print=FALSE}
# Load the libraries
library(tidyverse)
library(lubridate)
library(skimr)
library(janitor)
library(htmltools)
library(dplyr)
```


2. load dataset
```{r load datasets, message=FALSE, eval=FALSE}

dailyActivity_merged <- read_csv("R/Capstone/Case 2/Fitabase Data 3.12.16-4.11.16/dailyActivity_merged.csv")
dailyActivity_merged_2 <- read_csv("R/Capstone/Case 2/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
hourlySteps_merged <- read_csv("R/Capstone/Case 2/Fitabase Data 3.12.16-4.11.16/hourlySteps_merged.csv")
hourlySteps_merged_2 <- read_csv("R/Capstone/Case 2/Fitabase Data 4.12.16-5.12.16/hourlySteps_merged.csv")
hourlyCalories_merged <- read_csv("R/Capstone/Case 2/Fitabase Data 3.12.16-4.11.16/hourlyCalories_merged.csv")
hourlyCalories_merged_2 <- read_csv("R/Capstone/Case 2/Fitabase Data 4.12.16-5.12.16/hourlyCalories_merged.csv")
hourlyIntensities_merged <- read_csv("R/Capstone/Case 2/Fitabase Data 3.12.16-4.11.16/hourlyIntensities_merged.csv")
hourlyIntensities_merged_2 <- read_csv("R/Capstone/Case 2/Fitabase Data 4.12.16-5.12.16/hourlyIntensities_merged.csv")
sleepDay_merged_2 <- read_csv("R/Capstone/Case 2/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")

```


3. Inspect the data
```{r inspect dataset, eval=FALSE, results='hide'}
# View structure of datasets
glimpse(dailyActivity_merged)
glimpse(dailyActivity_merged_2)

# Check for missing values
sum(is.na(dailyActivity_merged))
sum(is.na(dailyActivity_merged_2))

# Summary statistics
skim(dailyActivity_merged)
skim(dailyActivity_merged_2)
```


4. Merge data set - Since some data set come from different folders but have the same structure, merge them.
```{r Merge dataset, eval=FALSE, results='hide'}

# combine relevant data set

daily_activity <- bind_rows(dailyActivity_merged, dailyActivity_merged_2)

hourly_steps <- bind_rows(hourlySteps_merged, hourlySteps_merged_2)

hourly_calories <- bind_rows(hourlyCalories_merged, hourlyCalories_merged_2)

hourly_intensities <- bind_rows(hourlyIntensities_merged, hourlyIntensities_merged_2)

# change data set name

sleep_data <- sleepDay_merged_2
```


5. Check data sets
```{r check dataset, eval=FALSE, results='hide'}

str(daily_activity)
str(hourly_steps)
str(hourly_calories)
str(hourly_intensities)
str(sleep_data)
```


6. Create and manipulate columns
```{r Create and manipulate columns, eval=FALSE, results='hide'}

# Convert ActivityDate to Date format and extract Month, Day, and Weekday
daily_activity <- daily_activity %>%
  mutate(ActivityDate = mdy(ActivityDate),
         Month = month(ActivityDate, label = TRUE),
         Day = day(ActivityDate),
         Weekday = wday(ActivityDate, label = TRUE, abbr = TRUE))  # Extracts weekday (e.g., "Mon", "Tue")

# Convert ActivityHour to Date-Time format and extract Month, Day, Weekday, and Time (24-hour format)
hourly_steps <- hourly_steps %>%
  mutate(ActivityHour = mdy_hms(ActivityHour),
         Month = month(ActivityHour, label = TRUE),
         Day = day(ActivityHour),
         Weekday = wday(ActivityHour, label = TRUE, abbr = TRUE),
         Time_24hr = format(ActivityHour, "%H:%M"))

hourly_calories <- hourly_calories %>%
  mutate(ActivityHour = mdy_hms(ActivityHour),
         Month = month(ActivityHour, label = TRUE),
         Day = day(ActivityHour),
         Weekday = wday(ActivityHour, label = TRUE, abbr = TRUE),
         Time_24hr = format(ActivityHour, "%H:%M"))

hourly_intensities <- hourly_intensities %>%
  mutate(ActivityHour = mdy_hms(ActivityHour),
         Month = month(ActivityHour, label = TRUE),
         Day = day(ActivityHour),
         Weekday = wday(ActivityHour, label = TRUE, abbr = TRUE),
         Time_24hr = format(ActivityHour, "%H:%M"))

# Convert SleepDay to Date-Time format and extract Month, Day, Weekday, and Time (24-hour format)
sleep_data <- sleep_data %>%
  mutate(SleepDay = mdy_hms(SleepDay),
         Month = month(SleepDay, label = TRUE),
         Day = day(SleepDay),
         Weekday = wday(SleepDay, label = TRUE, abbr = TRUE),
         Time_24hr = format(SleepDay, "%H:%M"))
```


II. Data Cleaning


```{r Data Cleaning, eval=FALSE, results='hide'}
# Function to clean datasets
clean_data <- function(data, datetime_col = NULL) {
  # Convert Date columns
  if (!is.null(datetime_col)) {
    data <- data %>%
      mutate(!!datetime_col := as.POSIXct(!!sym(datetime_col), format = "%Y-%m-%d %H:%M:%S", tz = "UTC"))
  }
  
  # Convert categorical columns
  if ("Month" %in% colnames(data)) {
    data$Month <- factor(data$Month, levels = month.name, ordered = TRUE)
  }
  
  if ("Weekday" %in% colnames(data)) {
    data$Weekday <- factor(data$Weekday, levels = c("Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"), ordered = TRUE)
  }

  # Check and remove duplicates
  data <- data %>% distinct()

  # Check and handle missing values
  if (sum(is.na(data)) > 0) {
    data <- na.omit(data)
  }
  
  return(data)
}

# Clean each dataset
daily_activity <- clean_data(daily_activity, "ActivityDate")
hourly_steps <- clean_data(hourly_steps, "ActivityHour")
hourly_calories <- clean_data(hourly_calories, "ActivityHour")
hourly_intensities <- clean_data(hourly_intensities, "ActivityHour")
sleep_data <- clean_data(sleep_data, "SleepDay")

# Convert Time_24hr to proper format if necessary
if ("Time_24hr" %in% colnames(hourly_steps)) {
  hourly_steps$Time_24hr <- format(strptime(hourly_steps$Time_24hr, "%H:%M"), "%H:%M")
}

if ("Time_24hr" %in% colnames(hourly_calories)) {
  hourly_calories$Time_24hr <- format(strptime(hourly_calories$Time_24hr, "%H:%M"), "%H:%M")
}

if ("Time_24hr" %in% colnames(hourly_intensities)) {
  hourly_intensities$Time_24hr <- format(strptime(hourly_intensities$Time_24hr, "%H:%M"), "%H:%M")
}

if ("Time_24hr" %in% colnames(sleep_data)) {
  sleep_data$Time_24hr <- format(strptime(sleep_data$Time_24hr, "%H:%M"), "%H:%M")
}

# View cleaned data structure
str(daily_activity)
str(hourly_steps)
str(hourly_calories)
str(hourly_intensities)
str(sleep_data)
```


Save Cleaned and Relevant data
```{r Save cleaned and relevant data, eval=FALSE, results='hide'}
write.csv(daily_activity,file = "~/R/Capstone/Case 2/Cleaned/daily_activity.csv",row.names = FALSE)
write.csv(hourly_steps,file = "~/R/Capstone/Case 2/Cleaned/hourly_steps.csv",row.names = FALSE)
write.csv(hourly_calories,file = "~/R/Capstone/Case 2/Cleaned/hourly_calories.csv",row.names = FALSE)
write.csv(hourly_intensities,file= "~/R/Capstone/Case 2/Cleaned/hourly_intensities.csv",row.names = FALSE)
write.csv(sleep_data,file = "~/R/Capstone/Case 2/Cleaned/sleep_data.csv",row.names = FALSE)
```


III Merge some data sets together


a. daily_activity and sleep_data as activity_sleep
```{r activity sleep, eval=FALSE, results='hide'}
# Ensure SleepDay is in Date format for correct merging
sleep_data <- sleep_data %>%
  mutate(SleepDay = as.Date(SleepDay)) %>%   # Convert SleepDay to Date format
  select(-Time_24hr)  # Remove Time_24hr column

# Merge with left join to retain all IDs from daily_activity
activity_sleep <- daily_activity %>%
  left_join(sleep_data, by = c("Id" = "Id", "ActivityDate" = "SleepDay")) %>%
  select(-Month.y, -Day.y, -Weekday.y) %>%   # Remove duplicate Month, Day, Weekday
  rename(Month = Month.x, Day = Day.x, Weekday = Weekday.x)  # Keep only daily_activity's Month, Day, Weekday

# Print structure to confirm merge
str(activity_sleep)

```

b. hourly_steps, hourly_calories and hourly_intensities as hourly_data
```{r hourly data, eval=FALSE, results='hide'}
# Load necessary library
library(dplyr)

# Merge hourly_steps and hourly_calories
hourly_data <- merge(hourly_steps, hourly_calories, 
                     by = c("Id", "ActivityHour"), 
                     all = TRUE, suffixes = c("_steps", "_calories"))

# Merge the result with hourly_intensities
hourly_data <- merge(hourly_data, hourly_intensities, 
                     by = c("Id", "ActivityHour"), 
                     all = TRUE, suffixes = c("", "_intensities"))

# Remove duplicate columns (keeping only one set of Month, Day, Weekday, and Time_24hr)
hourly_data <- hourly_data %>%
  select(Id, ActivityHour, StepTotal, Calories, TotalIntensity, AverageIntensity, 
         Month = Month_steps, Day = Day_steps, Weekday = Weekday_steps, Time_24hr = Time_24hr_steps)

# View final structure
str(hourly_data)

```


IV. Clean column names


```{r Clean column name, eval=FALSE, results='hide'}
# Clean column names for both datasets
activity_sleep <- clean_names(activity_sleep)
hourly_data <- clean_names(hourly_data)

# View cleaned column names
colnames(activity_sleep)
colnames(hourly_data)
```


V. Save cleaned data


```{r save cleaned data, eval=FALSE, results='hide'}

# save as csv

write.csv(activity_sleep,file = "~/R/Capstone/Case 2/Cleaned/activity_sleep.csv",row.names = FALSE)
write.csv(hourly_data,file = "~/R/Capstone/Case 2/Cleaned/hourly_data.csv",row.names = FALSE)

```


```{r load dataframe, message=FALSE, warning=FALSE, echo=FALSE}

activity_sleep <- read_csv("~/R/Capstone/Case 2/Cleaned/activity_sleep.csv")
hourly_data <- read_csv("~/R/Capstone/Case 2/Cleaned/hourly_data.csv")
```


### ANALYZE PHASE


**Exploratory Data Analysis (EDA)**

**A. Participant Counts**


**i.Number of unique participants in each dataset (Fitbit users in the study)**
```{r Participant counts, echo=FALSE, message=FALSE, warning=FALSE}
# Function to count unique users in a dataset
count_unique_users <- function(df, id_col) {
  return(length(unique(df[[id_col]])))
}

# Count unique users for each dataset (with corrected column name)
unique_users <- data.frame(
  Dataset = c("Activity_Sleep", "Hourly_Data"),
  Unique_Users = c(count_unique_users(activity_sleep, "id"),
                   count_unique_users(hourly_data, "id"))
)

# Print the results
print(unique_users)
```


**ii. Summary of Data Collection: Year, Months, and Days Recorded**
```{r Summary of Data Collection: Year, Months, and Days Recorded, echo=FALSE, message=FALSE, warning=FALSE}
# Extract year, unique months, and number of days from the dataset
summary_table <- activity_sleep %>%
  summarize(
    year = unique(format(activity_date, "%Y")),
    month = paste(unique(format(activity_date, "%B")), collapse = ", "),
    days_collected = n_distinct(activity_date)
  )

# Print the result as a table
print(summary_table)
```


**B. Observation Counts**



```{r Observation counts, echo=FALSE, message=FALSE, warning=FALSE}

# Create a data frame with observation counts for the two datasets
observation_counts <- data.frame(
  Dataset = c("Activity_Sleep", "Hourly_Data"),
  Observations = c(nrow(activity_sleep), nrow(hourly_data))
)

# Print the result
print(observation_counts)
```


**C. Variable Summaries**


a. Summary statistics for variables (TotalSteps, TotalDistance, and SedentaryMinutes) of daily activity data.
```{r Summary statistics 1, echo=FALSE, message=FALSE, warning=FALSE}

# Calculate min, max, and median for selected variables in activity_sleep dataset
summary_stats_activity_sleep <- data.frame(
  Variable = c("total_steps", "total_distance", "sedentary_minutes"),
  Min = c(min(activity_sleep$total_steps, na.rm = TRUE), 
          min(activity_sleep$total_distance, na.rm = TRUE), 
          min(activity_sleep$sedentary_minutes, na.rm = TRUE)),
  Max = c(max(activity_sleep$total_steps, na.rm = TRUE), 
          max(activity_sleep$total_distance, na.rm = TRUE), 
          max(activity_sleep$sedentary_minutes, na.rm = TRUE)),
  Median = c(median(activity_sleep$total_steps, na.rm = TRUE), 
             median(activity_sleep$total_distance, na.rm = TRUE), 
             median(activity_sleep$sedentary_minutes, na.rm = TRUE))
)

# Print results
print(summary_stats_activity_sleep)
```


b. Summary statistics for variables (TotalSleepRecords, TotalMinutesAsleep, and TotalTimeInBed) of daily activity data.
```{r Summary statistics 2, echo=FALSE, message=FALSE, warning=FALSE}

# Calculate min, max, and median for sleep-related variables in activity_sleep dataset
summary_stats_activity_sleep <- data.frame(
  Variable = c("total_sleep_records", "total_minutes_asleep", "total_time_in_bed"),
  Min = c(min(activity_sleep$total_sleep_records, na.rm = TRUE), 
          min(activity_sleep$total_minutes_asleep, na.rm = TRUE), 
          min(activity_sleep$total_time_in_bed, na.rm = TRUE)),
  Max = c(max(activity_sleep$total_sleep_records, na.rm = TRUE), 
          max(activity_sleep$total_minutes_asleep, na.rm = TRUE), 
          max(activity_sleep$total_time_in_bed, na.rm = TRUE)),
  Median = c(median(activity_sleep$total_sleep_records, na.rm = TRUE), 
             median(activity_sleep$total_minutes_asleep, na.rm = TRUE), 
             median(activity_sleep$total_time_in_bed, na.rm = TRUE))
)

# Print results
print(summary_stats_activity_sleep)
```


**D. Correlation analysis**


**1. Activity vs. Calories Burned**


[![Tableau Visualization](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/Activity%20vs.%20Calories%20Burned.png?raw=true)](https://public.tableau.com/views/CorrelationAnalysisforBellabeat/Activityvs_CaloriesBurned?:language=en-US&:embed=y&:sid=&:redirect=auth&:embed_code_version=3&:loadOrderID=0&:display_count=y&:origin=viz_share_link)



**Insights**


**a. Steps vs. Calories Burned (Scatter Plot with Trend Line)**


The majority of points are clustered at the lower-left corner of the plot, showing that many users have low step counts and low calorie burn.

The trend line tilts significantly to the left, moving upward, suggesting that as step count increases, calorie burn also rises.

The correlation is weak but statistically significant (r = 0.0660, p < 0.0001), meaning more steps contribute to higher calorie burn, but other factors also influence energy expenditure.


Walking plays a role in calorie burn, but it is not the sole determinant—other activity types also contribute.


**b. Total Active Minutes vs. Calories Burned (Bubble Chart)**


Bubble sizes vary, representing different calorie burn levels.

As active minutes increase (moving right), bubble sizes also increase, indicating that more active minutes generally lead to higher calorie expenditure.

However, some large bubbles appear on the left side, meaning that even short workouts can sometimes result in high calorie burn (possibly due to high-intensity activities).

The right side of the chart has a mix of small and large bubbles, suggesting that longer activity durations do not always guarantee higher calorie burn—intensity and type of activity matter.

Total active minutes strongly influence calorie burn, but intensity and activity type play a crucial role in determining energy expenditure.


**c. Sedentary Minutes vs. Calories Burned (Stacked Bar Chart)**

The bars show a fluctuating pattern rather than a clear trend.

Average calorie burn initially increases with sedentary minutes but then starts to decline after a certain threshold, forming an inverted U-shaped pattern.

The stacked bars alternate between different activity levels (sedentary, active, highly active), suggesting that some users compensate for high sedentary time with bursts of activity, while others remain mostly inactive.

Prolonged inactivity does not always lead to extremely low calorie burn, but after a certain point, excessive sedentary time reduces overall energy expenditure.


**Conclusion: Activity vs. Calories Burned**


The relationship between activity levels and calorie burn is influenced by multiple factors, including steps taken, total active minutes, and sedentary time.

More steps generally lead to higher calorie burn, but the correlation is weak. While increasing step count is beneficial, other factors like intensity and activity type also contribute significantly.

Total active minutes have a stronger impact on calorie expenditure, with longer active periods generally resulting in higher calorie burn. However, not all long-duration activities lead to high calorie loss—intensity plays a crucial role.

Sedentary time has a non-linear effect on calorie burn. Moderate sedentary time does not drastically reduce calorie expenditure, but excessive inactivity leads to lower overall energy burn. Some users compensate for inactivity with bursts of movement, while others remain mostly sedentary.


**Key Takeaways:**


Walking contributes to calorie burn, but its impact is limited without intensity.
Total activity time is a stronger predictor of calorie burn than step count alone.
Too much sedentary time reduces energy expenditure, though some active users offset this with high-intensity movement.


**2. Activity vs. Stress Levels**

[![Tableau Visualization](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/Activity%20vs.%20Stress%20Levels.png?raw=true)](https://public.tableau.com/views/CorrelationAnalysisforBellabeat/Activityvs_StressLevels?:language=en-US&:embed=y&:sid=&:redirect=auth&:embed_code_version=3&:loadOrderID=1&:display_count=y&:origin=viz_share_link)

**Insights**


**a. Steps vs. Total Intensities (Scatter Plot with Trend Line)**

Most points are clustered around the trend line, suggesting that step count is strongly related to total intensity levels.

The trend line tilts to the left and moves upward, showing that as steps increase, total intensity levels rise.

The correlation is strong (r = 0.8043, p < 0.0001), meaning that more walking is strongly associated with higher intensity levels.

More steps contribute significantly to higher intensity levels, reinforcing the importance of daily movement in maintaining activity intensity.


**b.Total Active Minutes vs. Total Intensities (Bubble Chart)**

The bubble chart reveals a clear intensity distribution:


Low-intensity activities are clustered at the bottom with smaller bubble sizes, indicating lower calorie burn.

Moderate-intensity activities are fewer in number and moderate in size.

High-intensity activities dominate the upper section, with bubbles of all sizes, indicating varied calorie burn rates at high intensity.

The rightward shift of bubbles suggests that longer workouts tend to involve higher intensities.

Larger bubbles represent higher calorie burn, which is more common in high-intensity activities but not guaranteed.

Longer workouts generally lead to higher intensity levels, but not all high-intensity workouts result in high calorie burn—indicating variations in workout effectiveness.


**c.  Sedentary Minutes vs. Total Intensities (Bar Chart)**


The bars show random heights and activity levels, meaning no clear pattern between sedentary time and total intensity levels.

This suggests that total sedentary minutes alone do not directly determine overall activity intensity.

Being sedentary does not automatically mean lower intensity levels, but prolonged inactivity could still indirectly reduce overall daily activity levels.


**Conclusion: Activity vs. Stress Levels**


The analysis reveals that activity levels have a strong influence on intensity, which can indicate physical stress or exertion levels.

Walking significantly increases intensity levels (r = 0.8043, p < 0.0001), meaning that users who take more steps tend to experience higher intensity activity. Walking is an effective way to maintain movement and increase exertion.

Longer workout durations are generally linked to higher intensity levels, as shown by the distribution of bubble sizes and colors. However, not all high-intensity workouts result in high calorie burn, indicating that the effectiveness of workouts varies.

Sedentary minutes do not show a direct relationship with intensity levels (random bar heights), but prolonged inactivity may still indirectly impact activity balance by reducing overall movement opportunities.


**Final Takeaway:**


Regular movement, especially walking and longer workouts contributes to higher intensity levels, while sedentary time alone does not dictate overall activity intensity.


**3. Activity vs. Sleep Patterns**

[![Tableau Visualization](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/Activity%20vs.%20Sleep%20Patterns.png?raw=true)](https://public.tableau.com/views/CorrelationAnalysisforBellabeat/Activityvs_SleepPatterns?:language=en-US&:embed=y&:sid=&:redirect=auth&:embed_code_version=3&:loadOrderID=2&:display_count=y&:origin=viz_share_link)

**Insights**


**a. Total Active Minutes vs. Sleep Duration (Scatter Plot with Trend Line)**


Data points are spread across the chart but concentrated along the trend line, indicating that most users fall within a typical range of activity and sleep duration.


Sleep quality patterns:


Poor sleep quality is mostly below the trend line, suggesting that less active users may experience lower sleep quality.

Moderate sleep quality is slightly below and on the trend line, meaning users in this category have varied activity levels but no strong trend with sleep duration.

Good sleep quality is on and above the trend line, implying that higher activity levels are associated with better sleep quality.

The trend line is nearly horizontal (r = 0.0068, p = 0.3228), meaning there is no strong correlation between total active minutes and sleep duration.

Total active minutes do not significantly impact sleep duration, but higher activity levels may contribute to better sleep quality.


**b. Steps vs. Sleep Duration (Scatter Plot with Trend Line)**


Points are scattered along the trend line, with very few points on the far right, indicating that only a small percentage of users take extremely high step counts.

Smaller points (representing fewer steps) are below the plot, meaning low step counts may be linked to shorter sleep duration.

Trend line tilts slightly to the right (r = 0.0643, p = 0.0020), showing a very weak but statistically significant positive correlation between higher step counts and longer sleep durations.

Users who take more steps tend to sleep slightly longer, but the effect is minimal.


**c. Sedentary Minutes vs. Sleep Duration (Bar Chart)**


The bar chart forms a distribution curve:

Initially, as sedentary minutes increase, sleep duration also increases.
However, beyond a certain threshold, too much sedentary time results in shorter sleep durations.

This suggests a U-shaped relationship between inactivity and sleep—moderate inactivity may be beneficial, but excessive sedentary behavior harms sleep.

A balanced amount of sedentary time may support sleep, but excessive inactivity reduces sleep duration, possibly due to reduced physical tiredness or disrupted sleep cycles.


The analysis of activity levels and sleep duration reveals that while physical activity (total active minutes and steps) has a weak positive correlation with sleep, the impact is minimal. In contrast, sedentary time exhibits a non-linear relationship, where moderate inactivity may support longer sleep durations, but excessive sedentary behavior appears to reduce sleep.


**Conclusion: Activity vs. Sleep Patterns**


The analysis indicates that activity levels have a weak but noticeable impact on sleep patterns, with moderate movement supporting better sleep quality and excessive inactivity reducing sleep duration.

Total Active Minutes vs. Sleep Duration: There is no strong correlation between total active minutes and sleep duration (r = 0.0068, p = 0.3228). However, users with higher activity levels tend to experience better sleep quality, suggesting that staying active positively influences rest.

Steps vs. Sleep Duration: A very weak but significant positive correlation exists (r = 0.0643, p = 0.0020), indicating that users who take more steps per day may sleep slightly longer. This suggests that regular movement, even at low intensities, may help improve sleep.

Sedentary Minutes vs. Sleep Duration: The U-shaped relationship shows that moderate sedentary time supports sleep, but excessive inactivity leads to shorter sleep durations. This may be due to a lack of physical exertion preventing restful sleep.


**Final Takeaway:**


Maintaining a balance between movement and rest is key to better sleep patterns. While total activity levels do not strongly influence sleep duration, moderate movement throughout the day and avoiding excessive sedentary behavior can help optimize sleep quality.


**4. Sleep vs. Calories Burned**

[![Tableau Visualization](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/Sleep%20vs.%20Calories%20Burned.png?raw=true)](https://public.tableau.com/shared/5F9PYWPGC?:display_count=y&:origin=viz_share_link&:embed=y)

**Insights**

**a. Sleep Duration vs. Calories Burned (Scatter Plot with Trend Line)**

The data points are scattered in both upward and downward directions, with higher concentration in the middle, indicating that most users have moderate sleep durations and varied calorie expenditure levels.

The trend line tilts slightly to the right, with r and p, suggesting no significant correlation between sleep duration and calories burned.

Despite the lack of strong correlation, the presence of high activity levels among users suggests that calorie burn is more dependent on physical activity than sleep duration.

Longer sleep duration does not necessarily lead to higher calorie expenditure. Caloric burn is likely influenced more by activity levels than by sleep length.



**b. Sleep Efficiency vs. Calories Burned (Bubble Chart)**

Most data points are concentrated on the right edge of the chart, showing that a large portion of users maintain high sleep efficiency regardless of calorie expenditure.

The upward and downward spread suggests variability in calorie burn among those with good sleep efficiency.

The presence of different bubble sizes, with many in the vigorous exercise intensity category, implies that high-calorie burn is more closely linked to intense physical activity rather than sleep efficiency alone.

Good sleep efficiency does not directly boost calorie expenditure, but individuals with higher activity levels and vigorous exercise tend to maintain better sleep efficiency.


**Conclusion: Sleep vs. Calories Burned**


The analysis shows that sleep duration and efficiency have minimal direct impact on calorie expenditure. Instead, physical activity plays a more significant role in determining how many calories an individual burns.

Sleep Duration and Calories Burned: There is no strong correlation between longer sleep and higher calorie burn (r = 0.0073, p = 0.303). This suggests that simply getting more sleep does not directly lead to increased calorie expenditure.

Sleep Efficiency and Calories Burned: While individuals with higher sleep efficiency are present across different calorie expenditure levels, the biggest factor influencing calorie burn is physical activity—especially vigorous exercise.


**Final Takeaway:**


Good sleep quality supports overall well-being and recovery, but it does not significantly boost calorie expenditure on its own. Instead, those who engage in vigorous physical activity tend to have better sleep efficiency and higher calorie burn.


**5.  Intensities vs. Sleep Quality (Stress Impact on Sleep)**

[![Tableau Visualization](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/_Intensities%20vs.%20Sleep%20Quality%20(Stress%20Impact%20on%20Sleep).png?raw=true)](https://public.tableau.com/views/CorrelationAnalysisforBellabeat/Intensitiesvs_SleepQualityStressImpactonSleep?:language=en-US&:embed=y&:sid=&:redirect=auth&:embed_code_version=3&:loadOrderID=4&:display_count=y&publish=yes&:origin=viz_share_link)

**Insights**

**a. Total Intensities vs. Sleep Duration (Scatter Plot with Trend Line)**

The data points are densely clustered in the middle, suggesting that most users engage in moderate total intensity levels rather than extremes.

The trend line tilts slightly downward to the right, with r and p, indicating a moderate negative correlation—higher total activity is linked to shorter sleep durations.

This suggests that while being active is beneficial, excessive physical activity might reduce sleep time, potentially due to factors like increased stress or difficulty winding down after intense exercise. While moderate activity supports sleep.


**b. Vigorous Intensity Minutes vs. Sleep Duration (Scatter Plot)**

The data points are concentrated on the left, indicating that most users do not engage in high levels of vigorous activity.

The trend line slants slightly to the right, with r and p, suggesting a weak but statistically significant positive correlation—vigorous activity has a small positive effect on sleep duration.

However, given the weak correlation, vigorous activity is not a major factor in improving sleep but may help slightly for those who engage in it.

High-intensity exercise does not significantly harm sleep duration and may slightly improve it, possibly by aiding deeper sleep.


**c. Light Intensity Minutes vs. Sleep Duration (Bubble Chart)**

The distribution mirrors the Total Intensities vs. Sleep Duration scatter plot, indicating a similar pattern between light activity and sleep duration. This may imply that light activities like walking or stretching do not significantly impact sleep negatively.

The bubbles are larger and more frequent, suggesting that light-intensity movement is more common among users.


Light physical activity is widely practiced and appears neutral or slightly beneficial for sleep.


**d. Sedentary Minutes vs. Sleep Duration (Bar Chart)**

The bar chart forms a distribution curve. This suggests that a moderate level of inactivity might aid sleep (possibly due to reduced physical stress), but excessive sedentary behavior eventually harms sleep quality and duration.

Being sedentary to a certain extent may support sleep, but too much inactivity negatively impacts sleep duration—possibly due to reduced physical tiredness or disrupted circadian rhythms.


**Conclusion: Intensities vs. Sleep Quality (Stress Impact on Sleep)**


The relationship between physical activity intensity and sleep quality highlights the complex interaction between movement, stress, and rest.

Total Activity and Sleep: Higher overall activity levels show a moderate negative correlation with sleep duration. While moderate exercise supports sleep, excessive activity may reduce sleep time, possibly due to increased physiological stress.

Vigorous Intensity and Sleep: Vigorous exercise has a weak but positive effect on sleep duration, indicating that high-intensity activity does not harm sleep and may slightly improve it by promoting deeper rest.

Light Intensity and Sleep: Light-intensity activities are the most common and appear neutral or slightly beneficial for sleep, making them a suitable choice for relaxation before bed.

Sedentary Behavior and Sleep: Sleep duration follows a U-shaped trend with sedentary time—moderate inactivity may support rest, but excessive sedentary behavior leads to shorter sleep duration, possibly due to reduced physical tiredness or disrupted sleep cycles.


**Final Takeaway:**


Balancing activity levels is key to improving sleep quality. Moderate exercise enhances sleep, while excessive activity or extreme inactivity can negatively impact rest. Encouraging light-intensity movement in the evening and avoiding prolonged sedentary periods before bedtime can help optimize sleep.


**6. Sedentary Behavior vs. Overall Health Metrics**

[![Tableau Visualization](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/Sedentary%20Behavior%20vs.%20Overall%20Health%20Metrics.png?raw=true)](https://public.tableau.com/views/CorrelationAnalysisforBellabeat/SedentaryBehaviorvs_OverallHealthMetrics?:language=en-US&:embed=y&:sid=&:redirect=auth&:embed_code_version=3&:loadOrderID=5&:display_count=y&publish=yes&:origin=viz_share_link)


**Insights**


**a. Sedentary Minutes vs. Calories Burned (Scatter Plot)**

The trend line has a slight negative tilt, indicating a weak inverse relationship between sedentary time and calorie expenditure. However, the correlation coefficient r and p suggest no statistically significant relationship.

Prolonged inactivity does not necessarily lead to significantly lower calorie expenditure. Other factors, such as non-exercise activity, metabolic rate, or exercise bouts, might play a role in calorie burning.


**b. Steps vs. Sleep Duration (Box Plot)**

The "High Sedentary" group has shorter sleep durations than the "Low Sedentary" group, indicating that increased inactivity may be associated with shorter sleep.

Sleep efficiency classification (Good, Moderate, Poor) shows a trend where:

High and Low sedentary groups primarily report Good sleep quality.

The Moderate sedentary group has a more diverse distribution of sleep quality (Good, Moderate, Poor).

The High Sedentary category has uniform statistics suggesting limited variability in sleep duration among highly sedentary individuals.

The Low Sedentary group has a broader range of sleep durations, indicating that lower inactivity correlates with greater variability and potentially longer sleep.

Higher sedentary behavior appears to be linked to shorter sleep duration, but not necessarily worse sleep efficiency. The moderate sedentary group experiences the most variation in sleep quality.


**c. Sedentary Minutes vs. Total Intensities (Heat Map)**

There is an inverse trend: as sedentary time increases, total daily intensity decreases, meaning more sedentary individuals engage in lower intensity activity overall.

Users with the lowest total daily intensity levels tend to be in the highest sedentary time bins, reinforcing the idea that more inactivity correlates with lower intensity engagement.

Higher sedentary time is linked to lower total physical activity intensity, reinforcing the concept that prolonged sitting discourages active movement.


**Conclusion: Sedentary Behavior vs. Overall Health Metrics**

The analysis of sedentary behavior in relation to calorie expenditure, sleep duration, and physical activity intensity reveals key insights into how prolonged inactivity affects overall health:

Calorie Expenditure: There is no significant relationship between sedentary time and calories burned, suggesting that factors beyond inactivity, such as metabolism and non-exercise activity, influence energy expenditure.

Sleep Duration & Quality: Higher sedentary behavior is associated with shorter sleep durations, though sleep efficiency remains relatively good across all groups. The moderate sedentary group shows the most varied sleep quality, indicating a potential threshold where inactivity begins to impact sleep patterns.

Physical Activity Intensity: More sedentary individuals engage in lower-intensity activities, with the user distribution forming an inverse curve—the higher the sedentary time, the fewer total intensity minutes recorded.


**Final Takeaway:**


While prolonged sedentary time does not significantly reduce calorie expenditure, it is linked to shorter sleep durations and lower overall physical activity intensity. Encouraging users to break up long periods of inactivity and engage in moderate to high-intensity movements can help mitigate potential negative health effects.


**E. User Behavior Analysis**

**1. Daily Activity Patterns**

[![Tableau Visualization](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/Daily%20Activity%20Patterns.png?raw=true)](https://public.tableau.com/views/UserBehaviorAnalysisfortheBellabeatcasestudy/DailyActivityPatterns?:language=en-US&:embed=y&:sid=&:redirect=auth&:embed_code_version=3&:loadOrderID=6&:display_count=y&publish=yes&:origin=viz_share_link)

**Insights**

**a. Daily Steps Distribution (Bar Chart)**


As steps increase, the number of users decreases, indicating a small active segment take a significantly high number of steps. This suggest a right-skewed distribution. Data shows a Sedentary Majority (30 users, 36%), Moderate Movers (22 + 19 users, 49%) and High Performers (12 users, 15%).

Encouraging users to transition from low to moderate activity could significantly improve overall fitness levels.

Challenges, gamification, or step-based incentives can help users move to the next step category.


**b. Active Minutes per User (Box Plot)**


The box shows the average active time (2136 mins) is above the median (1471 mins), indicating a right-skewed distribution. This indicates that while most users are moderately active and barely meet minimum movement thresholds, a few highly active users push the upper limits. There's a significant proportion of users are consistently active.

Users with less than 1,000 active minutes need motivation (fitness reminders, social challenges, or rewards).

Targeting the 25th–50th percentile group (905–1,471 mins) for improvement would have the biggest impact on overall user activity.


Identifying User Groups


Low-activity users: Below Q1 (≤ 905.5 min), representing the least engaged users.

Moderately active users: Between Q1 and Q3 (905.5 - 3034 min), forming the majority of users.

Highly active users: Above Q3 (≥ 3034 min), showing intense engagement.

Extreme users (approaching the upper whisker at 4597 min) likely represent fitness enthusiasts or users logging prolonged sessions.


**c. Activity Minutes by Weekday (Heat-map)**


Weekly Activity Trends
 
Highest Activity Days: Tuesday & Monday - suggesting users are more active at the start of the week. 

Midweek Drop: Wednesday & Thursday - Users might experience fatigue or become occupied with work/school responsibilities.

Lowest Activity Days (Weekend Slump): Saturday & Friday - Users may be resting, socializing, or engaging in non-tracked activities.

Sunday Rebound - Activity significantly increases on Sunday, possibly due to users making up for inactivity during Friday and Saturday.

Users are more active on weekdays than weekends, seen by significant weekend activity drop especially on Saturday. This indicates a need for targeted interventions (weekend fitness challenges, social workout reminders, or recreational activities) and Encouraging Saturday movement (via sports, hikes, or fun step challenges) could balance weekday and weekend activity.


 **Conclusion: Daily Activity Patterns**
 
 
Average User Activity: Approximately 6,200 steps per day (weighted average) with a median of 5,000 steps, and about 2,136 active minutes per week (~305 minutes per day).
 
Distribution: A significant majority (over 60%) of users are in the low-activity range, with only about 15% being highly active.

Weekday Trends: Activity peaks on Monday–Tuesday, declines through Friday–Saturday, with a modest rebound on Sunday.

Engagement & Interventions: The data indicate inconsistent and often low engagement, highlighting a need for personalized interventions, motivational challenges, and strategies to increase activity—especially on weekends—to improve long-term health outcomes.

This comprehensive analysis suggests that while a small group of users is very active, the overall activity levels are low. Personalizing recommendations and providing targeted incentives can help boost the activity levels and engagement of the majority.


**Final Takeaway:**


The analysis of daily activity patterns reveals significant variations in user engagement, with a majority of users being low-active or sedentary and only a small percentage maintaining high activity levels. Step counts and active minutes indicate a right-skewed distribution, where a few highly active individuals elevate the average, while most users engage in minimal movement. Personalized, targeted strategies, especially on weekends, could help raise overall daily activity, potentially improving long-term health outcomes.


**2. Hourly Trends**

[![Tableau Visualization](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/Hourly%20Trends.png?raw=true)](https://public.tableau.com/views/UserBehaviorAnalysisfortheBellabeatcasestudy/HourlyTrends?:language=en-US&:embed=y&:sid=&:redirect=auth&:embed_code_version=3&:loadOrderID=7&:display_count=y&publish=yes&:origin=viz_share_link)

**Insights**


**a. Hourly Step Count Trends (Line Chart)**


The hourly step count trends show a gradual accumulation across the day, with a nearly steady increase from early morning to late night.

The overall mean remains in the 7,400 to 7,750 steps range per hour in the summary line chart, indicating that on average, users maintain a relatively consistent activity rate throughout the day.

Hourly trends

Early Morning Low:
At the start of the day (around 12–3 AM), the cumulative step count is at its lowest, reflecting the period when most users are asleep or llimited movement.

Gradual Increase:
As the day progresses, the cumulative count steadily rises, indicating consistent activity buildup. This reflects morning routines, commutes, and regular daily movements.

Midday Activity Dip:
Between the morning and evening peaks (typically around 11 AM–2 PM), there’s often a modest plateau or dip in steps. This may be due to structured work/school schedules where movement is limited.

Late Afternoon/Evening Peak:
The count peaks around 5–6 PM, suggesting that a substantial portion of daily movement is completed in the late afternoon, likely due to after-work or post-dinner walks.

Plateau Towards Night:
From the early evening into the late night (from 7 PM to 11 PM), the curve remains relatively flat, indicating that few additional steps are accumulated during these hours.

Peak Activity Hours: 17:00 (5 PM) and 21:00 (9 PM). This suggests evening exercise or post-work walks.


Overall, while the differences between hours are modest, the chart highlights that users tend to accumulate most of their steps during the day with noticeable increases in the morning and late afternoon, and minimal movement during sleep hours. This suggests potential opportunities for interventions (like activity prompts) during the late afternoon to ensure users meet their daily step goals.


**b. Steps & Intensity by Hour (Heat-map)**


The heat-map reveals several key insights about how steps and activity intensity vary by hour and day:

Daily Variability:

Weekdays vs. Weekends:

Weekdays generally show higher and more consistent step totals during active hours compared to weekends. For instance, many weekdays (especially Tuesday through Wednesday) exhibit very high step totals and intensity in the late afternoon and early evening, while weekends tend to have lower totals at similar hours.

Peak Activity Hours:

Late Afternoon to Early Evening:

Across various days, hours between roughly 17:00 (5 PM) and 22:00 (10 PM) often record the highest step totals. On some days, like Tuesday or Wednesday, these peaks are remarkably high—suggesting that users are engaging in more dynamic, higher-intensity activities during these periods (which in turn would contribute to higher calorie burn).

Low Activity Periods:

Late Night/Early Morning:

The lowest step totals and intensity values are observed in the early hours (around 1–5 AM), reflecting typical sleep or rest periods.
Intensity Insights:

Energy Expenditure Peaks:

The “Total Intensity” values likely reflecting factors such as pace or the metabolic demand of movement are highest during the same late afternoon to early evening period. This suggests that these hours not only accumulate many steps but are also when users perform more vigorous activity, which is important for maximizing calorie burn.


**Conclusion: Hourly Trends**

Overall, while the differences between hours are modest, the chart highlights that users tend to accumulate most of their steps during the day with noticeable increases in the morning and late afternoon, and minimal movement during sleep hours.

The heat-map shows that users are most active during the late afternoon to early evening on weekdays, which likely contributes most to their overall calorie burn, while early mornings and weekends tend to be less active.
 

**Final Takeaway:**


The analysis of Hourly Trends shows the  importance of targeted movement strategies that align with users’ natural activity rhythms. By addressing sedentary periods and optimizing peak movement times, to aid users enhance their daily activity, burn more calories efficiently, and maintain a balanced lifestyle via:

Targeting Fitness Campaigns: Promotions or fitness challenges should focus on peak hours (4 PM - 7 PM) when people are most active.

Encouraging Morning Activity: Since morning steps are lower, strategies like morning walking groups or fitness reminders could boost early activity.


**3. Weekly Trends** 

[![Tableau Visualization](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/Weekly%20Trends.png?raw=true)](https://public.tableau.com/views/UserBehaviorAnalysisfortheBellabeatcasestudy/WeeklyTrends?:language=en-US&:embed=y&:sid=&:redirect=auth&:embed_code_version=3&:loadOrderID=8&:display_count=y&publish=yes&:origin=viz_share_link)

**a. Average Steps on Weekdays vs. Weekends (Bar Chart)**


Users take slightly more steps on weekdays than weekends, but the difference is minimal.
This suggests that workdays encourage more movement, but the drop on weekends is not significant.


**b. Daily Activity Patterns - Weekday vs. Weekend (Line Chart)**


Tuesday stands out as the most active day highest average step count indicating a peak in midweek activity., while Sunday is the least active reinforcing the idea that users slow down at the end of the week.


**c. Active Minutes by Weekday vs. Weekend (Stacked Bar Chart)**


Weekdays:


More “Very Active” and “Fairly Active” minutes, suggesting structured movement like workouts, commuting, or work-related activities.

“Lightly Active” minutes are higher, indicating general movement throughout the day.


Weekends:


Less “Very Active” and “Fairly Active” minutes, suggesting a drop in structured exercise.


More sedentary time, with an increase in passive movement rather than intentional workouts.


**Conclusion: Weekly Trends**


Activity levels are slightly higher on weekdays than weekends. This suggests that work and weekday routines contribute to more movement, whereas weekends see a slight decline in structured activity.


Tuesday is the most active day, while Sunday is the least active, this indicates midweek motivation or work-related movement. Sunday shows the lowest activity, likely due to rest and relaxation.


Activity type varies by day type.

Weekdays: Higher "Very Active" and "Fairly Active" minutes, suggesting structured workouts and active routines.

Weekends: More "Lightly Active" minutes, implying less intense movement and more sedentary time.


**Final Takeaway:**


Weekday routines help maintain activity levels, but steps drop slightly on weekends.

Encouraging weekend workouts or light movement (like walks or group activities) can balance the trend.

Sustaining midweek momentum, especially on Tuesday, can keep users engaged in fitness habits


**Comprehensive Insights on Fitbit Users Behavior Analysis**


From the analysis above, I have discovered;


i. Routine-Driven Activity: Users’ movement patterns are strongly influenced by daily and weekly routines, with structured activity during workdays and more relaxed behavior on weekends.

ii. Segmented User Base: A significant portion of users remain in low to moderate activity brackets, signaling an opportunity for personalized interventions to boost overall fitness levels.

iii. Targeted Engagement Opportunities:

Evening and Midweek Focus: Capitalize on natural peaks (evening hours and Tuesday) to promote additional challenges or rewards.

Weekend Activation: Develop weekend-specific strategies (like group challenges or leisure activity prompts) to counteract the dip in activity.


**F. Sleep Analysis**

1. Average sleep duration - compared to recommended 7-9 hours (bar chart)

[![Tableau Visualization](https://github.com/Dr-Irtwange-NB/Bellabeat-s-analysis-case-study-/blob/main/images/Average%20Sleep%20Duration%20vs.%20Recommended%207-9%20Hours.png?raw=true)](https://public.tableau.com/views/AverageSleepDurationvs_Recommended7-9Hours/AverageSleepDurationvs_Recommended7-9Hours?:language=en-US&:embed=y&:sid=&:redirect=auth&:embed_code_version=3&:loadOrderID=9&:display_count=y&publish=yes&:origin=viz_share_link)

**Insights**


Overall Trend:

The average sleep duration across the 12 days hovers around 7 hours (approximately 6.98 hours when computed), which is right at the lower end of the recommended 7–9 hours. This suggests that, on average, users are barely meeting the minimum sleep recommendation.


Day-to-Day Variability:

Higher Sleep Days:

Days such as Day 12 (7.37 hours), Day 11 (7.31 hours), Day 8 (7.38 hours), and Day 2 (7.39 hours) indicate that on several occasions, users achieve sleep durations that comfortably meet or exceed the minimum threshold.

Lower Sleep Days:

Days like Day 5 (6.03 hours), Day 7 (6.77 hours), and Day 3 (6.62 hours) show that on some days, sleep duration falls noticeably short of the recommended minimum.

This variability could suggest inconsistency in sleep habits or potential factors that occasionally disrupt adequate sleep.


Implications for User Well-being:

Although many days are close to or above 7 hours, the overall average slightly below the recommended minimum could have long-term implications on users health and recovery.
The variability in sleep duration underscores the opportunity for interventions such as sleep coaching, better sleep hygiene practices, or personalized reminders to promote a more consistent sleep schedule.


**Conclusion: Average sleep duration (compared to recommended 7-9 hours)**


Near Minimum Threshold:

Users average around 7 hours of sleep, which is at the lower edge of the recommended 7–9 hours.

Inconsistency in Sleep Habits:

Some days (e.g., Days 12, 11, 8, and 2) exceed 7.3–7.4 hours, while others (e.g., Days 5, 7, and 3) fall significantly short.

Implication:

The variability suggests inconsistent sleep routines that could affect long-term health and recovery, highlighting opportunities for sleep coaching and improved sleep hygiene.


As seen in D. Correlation Analysis:


**2. Correlation Between Sleep and Activity Levels:**

This insight found in **3. Activity vs. Sleep Patterns** – the analysis here includes correlation values (e.g., r = 0.0068 for active minutes and r = 0.0643 for steps) that illustrate how sleep duration relates to different activity metrics.


**3. Sleep Quality Patterns (e.g., do active users sleep better?):**

This insight is discussed in **3. Activity vs. Sleep Patterns** (where differences in sleep quality are noted along with activity levels) and further explored in **5. Intensities vs. Sleep Quality** (Stress Impact on Sleep), which examines how various intensities (light, vigorous, etc.) affect sleep quality.


**Comprehensive Insights on Fitbit users Sleep Analysis**


From the analysis above, I have discovered;

i. Inconsistent Sleep Patterns:

The average sleep duration hovers around 7 hours, right at the lower end of the recommended 7–9 hours.

Notable day-to-day variability shows that some days fall significantly short of the ideal range, highlighting an inconsistency in sleep habits that could affect long-term health and recovery.

ii. Activity and Sleep Relationship:

While total active minutes do not show a strong correlation with sleep duration, a weak positive association exists between step counts and longer sleep.

Sedentary behavior displays a U-shaped relationship with sleep: moderate sedentary time may support better sleep, but excessive inactivity is linked with shorter sleep durations.

iii. Impact of Activity Intensity on Sleep Quality:

Higher overall activity intensity tends to correlate moderately with shorter sleep durations, potentially due to increased physical stress.

In contrast, engaging in vigorous activity shows a slight positive effect on sleep duration, suggesting that high-intensity exercise may promote deeper, more restorative sleep.

Light-intensity activities are common and appear neutral or slightly beneficial, offering a balanced approach to supporting sleep quality.


**Final Takeaway:**


Balancing moderate physical activity with adequate rest is key to optimizing sleep quality. Personalized sleep coaching and interventions that encourage consistent sleep routines, promote moderate exercise, and prevent excessive sedentary behavior can help enhance overall well-being among Fitbit users.


**G. Calorie Burn & Intensity Analysis**

As seen in D. Correlation Analysis:

**1. Daily Calorie Burn Trends:**

Found in **1. Activity vs. Calories Burned** where the scatter plots (Steps vs. Calories Burned) and bubble charts (Total Active Minutes vs. Calories Burned) illustrate how daily activity levels relate to energy expenditure.

**2. Relationship Between Intensity and Calories Burned:**

Also primarily in **1. Activity vs. Calories Burned** (via the Total Active Minutes analysis) and further supported by **2. Activity vs. Stress Levels** which shows how higher intensities (from increased steps and vigorous activity) correlate with greater energy output.

**3. Impact of Activity Levels on Calorie Burn:**

Detailed in **1. Activity vs. Calories Burned** where different activity metrics (steps, active minutes, sedentary minutes) are examined, and reinforced by insights in **Sleep vs. Calories Burned** and **6. Sedentary Behavior vs. Overall Health Metrics** which discuss how overall movement (or inactivity) affects calorie expenditure.


**Comprehensive Insights on Fitbit users Calorie Burn & Intensity Analysis**


From the analysis above, I have discovered:

i. Daily Calorie Burn Trends:

The "Activity vs. Calories Burned" section reveals that most users cluster at lower step counts and calorie expenditure levels, with increases in steps and active minutes generally leading to higher calorie burn.

The scatter plots and bubble charts illustrate that while many users maintain low daily calorie burn, periods of increased activity (both in duration and intensity) push energy expenditure upward.

ii. Relationship Between Intensity and Calories Burned:

Insights from both the "Activity vs. Calories Burned" and "Activity vs. Stress Levels" sections show that higher intensity—especially vigorous activity—drives a greater calorie burn compared to moderate activities.

Although the link between overall movement and energy expenditure is present, it is the quality (intensity) of the activity that markedly boosts calorie burn.

iii. Impact of Activity Levels on Calorie Burn:

While more steps and longer active minutes generally correspond with higher calorie burn, the correlation is weak, indicating that the type of activity and its intensity are also critical factors.

The analysis suggests that even with similar active minutes, users engaging in high-intensity workouts tend to burn more calories than those with lower intensity or prolonged sedentary intervals.


**Final Takeaway:**


Regular movement does contribute to increased calorie burn; however, optimizing energy expenditure relies on balancing both the duration and intensity of activity. A strategy that encourages not just more movement, but also higher-intensity exercises while reducing prolonged sedentary periods, will be most effective for improving calorie burn among Fitbit users.


**H. Sedentary Lifestyle Insights**


As seen in D. Correlation Analysis:


**1. Time spent sedentary vs. active:** 

Found in "**1. Activity vs. Calories Burned**" (specifically in the “Sedentary Minutes vs. Calories Burned (Stacked Bar Chart)” section) and also in "**3. Activity vs. Sleep Patterns**" (in the “Sedentary Minutes vs. Sleep Duration” section). These parts illustrate how daily activity levels (or lack thereof) relate to energy expenditure and sleep outcomes.

**2. Proportion of users with a sedentary lifestyle:**

Found in "**1. Daily Activity Patterns**" where users are segmented into groups (Sedentary Majority, Moderate Movers, and High Performers).


**3. Impact of sedentary time on health metrics:**

Found in "**6. Sedentary Behavior vs. Overall Health Metrics**". This section details how increased sedentary minutes are associated with lower physical activity intensity, shorter sleep durations, and their overall effects on health.


**Comprehensive Insights on Fitbit users Sedentary Lifestyle Analysis**


From the analysis above, I have discovered:

i. Time Spent Sedentary vs. Active

The data reveals that users often accumulate extended sedentary periods compared to their active minutes.

Visuals like the Sedentary Minutes vs. Calories Burned and Sedentary Minutes vs. Sleep Duration charts illustrate that, after a moderate level, increased inactivity tends to reduce physical activity intensity and may negatively impact energy expenditure and sleep.

ii. Proportion of Users with a Sedentary Lifestyle

User segmentation indicates that a significant majority of users fall within the low-activity or sedentary brackets.

For example, the daily activity patterns show that over 60% of users are categorized as low-active, underlining the prevalence of sedentary behavior among Fitbit users.

iii. Impact of Sedentary Time on Health Metrics

Although sedentary time does not have a strong direct correlation with calories burned, its indirect effects are notable.

Increased sedentary behavior is linked to lower overall physical activity intensity and shorter sleep durations, suggesting that prolonged inactivity can compromise both energy expenditure and sleep quality.


**Final Takeaway:**


Reducing extended periods of inactivity by incorporating regular active breaks and promoting moderate to vigorous physical activities can mitigate the negative health impacts of a sedentary lifestyle. Tailored interventions that focus on increasing overall movement and activity intensity are essential for improving the long-term well-being of Fitbit users.


#### **Key Insights & Business Recommendations for Bellabeat**

Based on analysis of Fitbit users, the following recommendation has been made for Bellabeat users.


1. Target Audience Identification

Insight: Users can be grouped into highly active, moderately active, and inactive users based on their daily step count and activity levels. Highly active users engage consistently, while inactive users show low movement patterns throughout the day.

Business Recommendation:

Primary target: Moderately active users — these users have the potential to become more engaged with the right motivation.

Secondary target: Inactive users — Bellabeat should introduce gentle nudges and smart reminders to help them integrate movement into their daily routines.


2. Product Design Suggestions

Insight: Data shows that activity tracking is the most consistently used feature, followed by calorie tracking, while sleep tracking has a lot of untapped potential due to inconsistent usage.

Business Recommendation:

Bellabeat should prioritize activity tracking improvements (e.g., personalized step goals, streak-based motivation).

Enhance sleep monitoring by integrating it with daily activity insights (e.g., "Your low activity today might affect your sleep quality tonight. Try a light walk before bed!").

Calorie tracking can be refined with nutritional recommendations based on daily activity levels.


3. Marketing Strategy

a. When should Bellabeat push notifications or reminders for workouts?

Insight: Users show peak activity in the evening (5-8 PM) and midweek (Tues-Thurs), with a drop in activity on weekends.

Business Recommendation:

Evening reminders (5-7 PM): Push workout reminders when users are most likely to engage.

Midweek motivation challenges (Tues-Thurs): Introduce mini-challenges to keep users consistent.

Weekend activation campaigns: Encourage users to stay active on weekends with step-based rewards or group fitness challenges.


b. What kind of users should be targeted for engagement campaigns?

Insight: Highly active users are self-motivated, while inactive users need structured support.

Business Recommendation:

For inactive users: Implement motivational nudges, such as "Start small! A 5-minute walk can boost your energy."

For moderately active users: Use streak-based challenges to build habits (e.g., “You're on a 3-day streak! Keep going to hit 5 days!”).

For highly active users: Offer leader-board rankings, advanced goal setting, or social sharing features to maintain engagement.


4. Potential Feature Improvements

a. Smart reminders for inactive users

Insight: Inactive users do not engage with activity tracking features as frequently.

Business Recommendation: Introduce adaptive reminders based on inactivity trends, such as “You've been sitting for 2 hours—try a 2-minute stretch!”

b. Personalized insights based on sleep & activity correlation

Insight: Users with irregular sleep patterns tend to have lower activity levels the next day.

Business Recommendation:

Integrate sleep and activity insights into daily recommendations (e.g., “Poor sleep detected last night—consider a light morning workout to boost energy!”).

Introduce a ‘Sleep & Energy Score’ that helps users understand how movement impacts sleep quality.


#### **Conclusion**


By implementing these recommendations, Bellabeat can:

1. Increase user engagement with targeted notifications and activity-based challenges.

2. Improve product appeal by enhancing activity tracking and sleep monitoring features.

3. Boost customer retention by providing personalized insights and smart reminders tailored to different user activity levels.


### SHARE PHASE

Presentation [here](https://rpubs.com/nguwasenirtwange/1283502).
