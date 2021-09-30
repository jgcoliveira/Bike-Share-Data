# Bike Share Service Users

**Bike Sharing Service Data BI project using using R (tidyverse packages) for data cleaning, and manipulation and visualization**

This Case Study is the Capstone Project of the Google Data Analytics Professional Certificate, meant to demonstrate the technical and analytical skills necessary for an entry-level Data Analyst position.

##Summary

* [1. Business Task](##1.-business-task)
* [2. Data Collection and Preparation](##2.-data-collection-and-preparation)
* [3. Data Cleaning and Manipulation](##3.-data-cleaning-and-manipulation)
* [4. Summary of Analysis](##4.-summary-of-analysis)
* [5. Key Findings and Supporting Visualizations](##5.-key-Findings-and-supporting-visualizations)
 
## 1. Business Task

### Purpose

In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Lily Moreno (Director of Marketing) believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

Design marketing strategies aimed at converting casual riders into annual members by: 
  better understanding how annual members and casual riders differ
  why casual riders would buy a membership
  how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.

### Scope

Considering that annual members are much more profitable than casual riders, we will analyze Cyclistic historical bike trip data to identify trends and to better understand the how different customer types use the Cyclistic bike service, so that we can present valuable insights to the marketing team that will facilitate the conversion of casual riders into annual members and consequently increase profit.

Are there any trends in the data?
In what ways do casual riders and annual members differ? What do they have in common?
Is there a connection between specific locations or routes and type of subscribers?
What are the Docking Station hotspots?
Are there patterns in the data? Weekly, monthly or annual? Subscription-wise?

This project does not include an in-depth look into why casual riders would buy a membership and how digital media could affect Cyclistic's marketing tactics.

### Deliverables and Milestones

Data Review
Data Cleaning
Data Analysis
Visualizations and Key Findings
Recommendations

## 2. Data Collection and Preparation

### Sources

We will use 12 months of Cyclistic trip data available through [this link](https://divvy-tripdata.s3.amazonaws.com/index.html). The data has been made available by Motivate International Inc. under [this license](https://www.divvybikes.com/data-license-agreement) for the purpose of this case study.

The data is divided across 4 folders, each one containing a csv file of quarterly trip data. Together they represent the 4 quarters of 2019 trip data.

For analysis purposes, the csv files will be merged onto 1 single file to facilitate the analysis

### Tools Used

Since we are dealing solely with equaly structured tabular data and there is no need to create a data schema, we will be using RStudio for the several tasks performed throughout this project.

### Columns

In order to combine all the dataframes into one, all must share the same columns with the same exact names (although not in the same order)

![image](https://user-images.githubusercontent.com/78386715/135453486-7b3954a0-ae8e-42e9-ae3d-02ba6a68de02.png)

 At first glance, we're able to see some incongruencies:
 - Q2_2019 and Q3_2019 share column names;
 - Q1_2020 does not have a gender nor a birthday column;
 - Q2_2019, Q3_2019 and Q4_2019 do not have latitude and longitude columns.

Assuming it will be the chosen table format going forward, we renamed all the columns and changed their data types to make them consistent with the Q12020 data frame.
Only then were we able to append the 4 data frames into a single one, and then after we deleted all the unused columns.

### Data Quality

Given its direct source, we can assume that the data is reliable, original, comprehensive and current.

Nevertheless, we'll review the data integrity in the Data Cleaning chapter by ensuring it is accurate, complete, consistent, trustworthy and aligned with the business objective before beginning to analyse it in order to draw conclusions.

## 3. Data Cleaning and Manipulation

By inspecting the data, we learn what tasks needed to be performed, and then proceeded to:

- remove unwanted characters and duplicate entries
```{r}
clean_names(Y_Cyclistic)

Y_Cyclistic %>% 
  distinct() %>% 
  nrow()
```

- fix inconsistent data by making sure there were no equivelent user types with different names, i.e. "member"/"Subscriber" and  "Customer"/"casual"
```{r}
Y_Cyclistic <- Y_Cyclistic %>%
  mutate(member_casual = recode(member_casual,
                                "Subscriber" = "member",
                                "Customer" = "casual"
                                )
         )
```

- include new calculated calendar columns (month, day, year, weekday) and trip-related columns (start hour, ride length)
```{r}
#calendar columns
Y_Cyclistic$date <- as.Date(Y_Cyclistic$started_at)
Y_Cyclistic$month <-format(as.Date(Y_Cyclistic$date), "%m")
Y_Cyclistic$day <-format(as.Date(Y_Cyclistic$date), "%d")
Y_Cyclistic$year <-format(as.Date(Y_Cyclistic$date), "%Y")
Y_Cyclistic$weekday <-format(as.Date(Y_Cyclistic$date), "%A")

#start hour column
Y_Cyclistic$start_hour <- hour(Y_Cyclistic$started_at)

#ride length 
Y_Cyclistic$ride_length <- difftime(strptime(Y_Cyclistic$ended_at, "%Y-%m-%d %H:%M:%S"), #ended_at needs to be first converted to time data type
                                    strptime(Y_Cyclistic$started_at, "%Y-%m-%d %H:%M:%S") #ended_at needs to be first converted to time data type
                                    )
```

- remove maintenance trips (having start_station_name == "HQ QR"), zero and negative time trips (ride_length <= 0)
```{r}
Y_Cyclistic_V2 <- Y_Cyclistic[!(Y_Cyclistic$start_station_name == "HQ QR" | Y_Cyclistic$ride_length <= 0),]
```

## 4. Summary of Analysis

As the goal of this project is to better understand the how different customer types use the Cyclistic bike service, let's start by looking at some summary statistics:
![image](https://user-images.githubusercontent.com/78386715/135455443-2bb8071e-8664-441e-8bf3-968bb637ebc0.png)

Let's also review the average duration and number of rides during the course of the week:
```{r}
Y_Cyclistic_V2 %>% 
  drop_na() %>% 
  group_by(weekday, member_casual) %>% 
  summarize( avg_duration = mean(ride_length), number_of_rides = n()) %>% 
  arrange(member_casual, weekday)
```
![image](https://user-images.githubusercontent.com/78386715/135455695-5336f36d-622a-4aee-bb09-a10d1fb34298.png)

Looking at the above data, it is noticeable that casual members take longer trips than members (+ 4x). But members perform a bigger amount of trips. We'll have to create graphs in order to better understand if there are any trends in the data.

## 5. Key Findings and Supporting Visualizations

Using the available data, we're able to plot several graphs, analyse them and draw some conclusions.

### Total Number of Rides per Weekday, User Type
![image](https://user-images.githubusercontent.com/78386715/135473312-bc8930bb-8ede-47a5-be11-8a4c8b5a9780.png)

**Findings**
  - Member trips increase at the start of the week, peaking during the middle of the week (wednesday) and then decreasing
  - Casual trips have the reverse trend, peaking during the weekend and then decreasing during the week
  - The amount of member trips continuously supersedes the amount of casual trips

### Average Ride Duration per Weekday, User Type
![image](https://user-images.githubusercontent.com/78386715/135474165-5cf3d75b-5ddd-4e18-83ae-f9e8107ace48.png)

**Findings**
  - On a daily basis, the average duration of casual trips tends to be approximately 4 longer than member trips.
  - Member trips seem to always be averaging 900 sec = 15 mins 
  - Casual trips always exceed the 3000 sec = 50 min mark, peaking on friday
  - Members' average ride duration does not fluctuate significantly during the course of the work week, only increasing slightly during the weekend
  - Casual riders seem to take longer trips during wednesday and friday

### Total Number of Rides per Hour, User Type
![image](https://user-images.githubusercontent.com/78386715/135475120-ca90ab8e-b4bd-4434-a615-436cb3d69015.png)

**Findings**
 - Members usually outperform casuals in terms of trip numbers, except for the early morning hours (0-5AM), during which both groups perform a similarly small number of trips
 - Members perform a bigger number of rides during commuting hours (7-8AM and 4-5PM)
 - The number of casual trips increases during the morning period, stabilizing from 11AM to 5PM, and then decreasing similarly to the morning rate
