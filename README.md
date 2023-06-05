# **Google Data Analytics Capstone Project - Cyclistics Bikes**

## **Project Overview**

Goal: To understand the differences in how annual members and casual riders use Cyclistic’s bikes and provide insights into their usage patterns, preferences, and behaviors. These insights will help the marketing team design strategies to convert more casual riders into annual members.

### **Data Overview**

* 5,800 bicycles and 600 docking stations installed around Chicago. <br>
* [Dataset](https://divvy-tripdata.s3.amazonaws.com/index.htm) consists of 12 CSV files each per month with 13 columns and 5,859,061 rows. <br>
* Data is stored in Amazon Web Server and is owned by Motivate International Inc under this [license](https://ride.divvybikes.com/data-license-agreement).

### ROCCC approach is used to determine the credibility of the data


* Reliable: The data is relevant, complete, consistent and it represents all bike rides taken in the city of Chicago for the selected duration of our analysis.
* Original: The data is made available by Motivate International Inc. which operates the city of Chicago.
* Comprehensive: The data covers a wide range of relevant variables, details including starting time, ending time, station name, station ID, type of membership and many more.
* Current: It is up-to-date as it includes data until end of April 2023. The data is cited and is available under Data License Agreement.

### **Data Limitation**
* Lack of demographic data like age or gender.<br>
* Absence of membership prices.<br>
* 27% of all electric bikes are missing the start station name.<br>

## **Prepare**
R is well-suited for cleaning and processing large datasets, data for this project has more than 5 million rows, so because of Its efficient memory management, vectorized operations, and extensive packages I have chosen R for cleaning and processing. Microsoft Excel to create the charts and Power Point to create the presentation.

### Installing and loading necessary packages
 ```ruby
install.packages("tidyverse")
install.packages("dplyr")
install.packages("writexl")

library(tidyverse)
library(dplyr)
library(writexl)
```
### Importing data to R Studio
```ruby
May_2022 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202205-divvy-tripdata.csv")
June_2022 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202206-divvy-tripdata.csv")
July_2022 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202207-divvy-tripdata.csv")
August_2022 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202208-divvy-tripdata.csv")
September_2022 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202209-divvy-publictripdata.csv")
October_2022 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202210-divvy-tripdata.csv")
November_2022 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202211-divvy-tripdata.csv")
December_2022 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202212-divvy-tripdata.csv")
January_2023 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202301-divvy-tripdata.csv")
February_2023 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202302-divvy-tripdata.csv")
March_2023 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202303-divvy-tripdata.csv")
April_2023 <- read_csv("G:\\My Drive\\Data Analytics\\Capstone Project\\Data\\Tripdata\\202304-divvy-tripdata.csv")
```
### Merging data into a data frame
```ruby
trip_data <- rbind(May_2022, June_2022, July_2022, August_2022, September_2022, October_2022, November_2022, December_2022, January_2023, February_2023, March_2023, April_2023)
```
## Process - Cleaning

### Reviewing 
```ruby
nrow(trip_data)
ncol(trip_data)
head(trip_data)
tail(trip_data)
str(trip_data)
summary(trip_data)
colnames(trip_data)
```
### Removing NA's
```ruby
sum(is.na(trip_data))
trip_data <- na.omit(trip_data)
```

### Adding new columns for duration, distance, year, month, day of week and hour of day
```ruby
trip_data_v2 <- trip_data %>% 
mutate(ride_duration = difftime(ended_at, started_at, units = "mins")) %>% 
mutate(ride_distance = distHaversine(cbind(start_lng, start_lat), cbind(end_lng, end_lat))) %>% 
mutate(ride_year = year(started_at)) %>% 
mutate(ride_month = month(started_at, label = TRUE)) %>% 
mutate(day_of_week = weekdays(started_at)) %>% 
mutate(hour_of_day = hour(started_at))
```
### Cleaning data

      ##Trips w/o start station
      ##Trips w/o ending station
      ##Trips with duration > 0
      ##Trips with distance > 0
```ruby
clean_data <- trip_data_v2 %>% 
  filter(!is.na(start_station_name)) %>% 
  filter(!is.na(end_station_name)) %>% 
  filter(ride_duration >0) %>% 
  filter(ride_distance >0)
```
### Double checking cleaned data
```ruby
nrow(subset(clean_data, start_station_name < 0))
nrow(subset(clean_data, end_station_name < 0))
nrow(subset(clean_data, ride_distance < 0))
nrow(subset(clean_data, ride_duration < 0))
```

## **Analysis**

* Classic Bikes are more popular.
* Saturday is the busiest day in general
* Weekends are the busiest days for casuals, but members tend to use the bikes more during the week.
* Summer and Autumn are the busiest seasons.

<p align="center">
  <img src= charts/Member%20vs%20Casuals.png width="600"/> </p> <br>
  
 <p align="center">
  <img src= charts/Bike%20Preference.png width="600"/> </p><br>
  
  <p align="center">
  <img src= charts/Weekly_Usage.png width="600"/> </p><br>
  
  <p align="center">
  <img src= charts/Season.png width="600"/> </p><br>
  
  <p float="left">
  <img src= charts/Avg_Duration.png width="500"/>
  <img src= charts/Avg_Distance.png width="500"/>
  </p>


  
 

## Key Insights

## Conclusions
