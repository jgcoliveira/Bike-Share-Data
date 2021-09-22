# Bike-Share-Data
Analysing previous bike sharing data in order to increase revenue

This Case Study is the Capstone Project of the Google Data Analytics Professional Certificate, meant to demonstrate the technical and analytical skills necessary for an entry-level Data Analyst position.

##Summary

1. Business Task
2. Data Sources
3. Data Cleaning and Manipulation
4. Summary of Analysis
5. Key Findings and Supporting Visualizations
6. Recommendations

  
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

## 2. Data Sources

### Location

We will use 12 months of Cyclistic trip data available through [this link](https://divvy-tripdata.s3.amazonaws.com/index.html). The data has been made available by Motivate International Inc. under [this license](https://www.divvybikes.com/data-license-agreement) for the purpose of this case study.

The data is divided across 4 folders, each one containing a csv file of quarterly trip data. Together they represent the 4 quarters of 2019 trip data.

For analysis purposes, the csv files will be merged onto 1 single file to facilitate the analysis

### Tools Used

Since we are dealing solely with equaly structured tabular data and there is no need to create a data schema, we will be using RStudio for the several tasks performed throughout this project.

### Columns

Let's look into the data
```R
install.packages("tidyverse")
library(tidyverse)

# load the first data set 
Q12019_Cyclistic <- read.csv("C:/Users/jguic/OneDrive/Desktop/Coding/Google Capstone/Case Study 1 - Cyclistic/Raw Data/Divvy_Trips_2019_Q1/Divvy_Trips_2019_Q1.csv")

# check the column names find
head(Q12019_Cyclistic)
```
![image](https://user-images.githubusercontent.com/78386715/134204550-28fd686b-0f91-41ce-b209-b68023015108.png)

### Data Quality

Given its direct source, we can assume that the data is reliable, original, comprehensive and current.

Nevertheless, we'll review the data integrity in the Data Cleaning chapter by ensuring it is accurate, complete, consistent, trustworthy and aligned with the business objective before beginning to analyse it in order to draw conclusions.

## 3. Data Cleaning and Manipulation


