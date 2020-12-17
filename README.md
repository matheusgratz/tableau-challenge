## Citibike Challenge using Tableau
#### Matheus Gratz
---
<strong>Agenda</strong>
- Introduction
  a) Task
  b) Personal Objectives
- Steps
  1) Gather the data
  2) Concatenate (Extract)
  3) Summarize (Transform)
  4) Export (Load)
- Links to Tableau Public
---
#### Introduction

##### Task
The task in this assignment is to aggregate the data found in the Citi Bike Trip History Logs and find two unexpected phenomena.

Design 2-5 visualizations for each discovered phenomena (4-10 total). You may work with a timespan of your choosing.

##### Objectives

Selected Timespan: All trips generated in 2019
Total trips: 20,551,517 trips

Question to be answered:
1) Total trips per week and season. Does the methereological season (and, by that the average temperature) impact on the total trips?
2) What is the distribution of trips by gender?
3) What is the distribution of trips by user type?
4) What is/are the most relevant stations, taking into account the total trips started on that station?
5) What is/are the most relevant routes (From Start Station - To End Station)?

##### Gather the data
[CitiBike Data Page](https://s3.amazonaws.com/tripdata/index.html)
* 201901-citibike-tripdata.csv
* 201902-citibike-tripdata.csv
* 201903-citibike-tripdata.csv
* 201904-citibike-tripdata.csv
* 201905-citibike-tripdata.csv
* 201906-citibike-tripdata.csv
* 201907-citibike-tripdata.csv
* 201908-citibike-tripdata.csv
* 201909-citibike-tripdata.csv
* 201910-citibike-tripdata.csv
* 201911-citibike-tripdata.csv
* 201912-citibike-tripdata.csv

##### Concatenate

This piece of code represent the concatenate process. For full code, please check the notebook [here]().

```python
extension = 'csv'
all_filenames = [i for i in glob.glob('*.{}'.format(extension))]
combined_df = pd.concat([pd.read_csv(f) for f in all_filenames ])
```

##### Summarize
First we need to convert the columns with date-time as a string to a real date-time type.
```python
# Split string date column to take out decimals
new_datetime = combined_df['starttime'].str.split(".", n = 1, expand = True)

# Set the columns value
combined_df['starttime'] = new_datetime[0]

# Convert to Datetime
combined_df['starttime'] = pd.to_datetime(combined_df['starttime'])
```

After that, create the week number (from 1 to 52)

```python
# Calculate the week number (from 1 to 52)
combined_df['weeknum'] = combined_df['starttime'].dt.isocalendar().week
```

And create the summaries.
a) By Weeknumber

```python
# Create a summary dataframe  by week number and some other important fields
weeknum_summary = combined_df.groupby(['start station id', 'start station name', 'start station latitude', 'start station longitude', 'usertype', 'gender', 'weeknum']).agg(
    total_trips = ('bikeid', 'count'),
    avg_trip_duration = ('tripduration', 'mean'),
    med_trip_duration = ('tripduration', 'median')  
    
)
```

b) From-To Station Summary

```python
# Create a summary dataframe from to stations
fromto_summary = combined_df.groupby(['start station id', 'start station name', 'start station latitude', 'start station longitude', 
                                      'end station id', 'end station name', 'end station latitude', 'end station longitude']).agg(
    total_trips = ('bikeid', 'count'),
    avg_trip_duration = ('tripduration', 'mean'),
    med_trip_duration = ('tripduration', 'median')      
)
```

##### Export

Generate the CSV Files:

```python
# Convert to CSV
weeknum_summary.to_csv('weeknum_summary.csv')
fromto_summary.to_csv('from_to_stations.csv')
```

##### Link to Tableau Public
* [Story](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/CitiBike-MGratz)
* [Seasons Dashboard](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/SeasonDashboard)
* [Gender Dashboard](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/GenderDashboard)
* [User Type Dashboard](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/UserTypeDashboard)
* [Relevant Stations Dashboard](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/RelevantStations-TotalTrips)
* [Relevant Routes Dashboard](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/RelevantRoutes-FromToStationsTotalTrips)
* Worksheets
	
  * [Weekly Total Trips](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/WeeklyTotalTrips)
  * [Total trips per Season](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/TotaltripsperSeason)
  * [Total Trips by Gender](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/TotalTripsbyGender)
  * [Weekly Trips by Gender](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/WeeklyTripsbyGender)
  * [Total Trips by User Type](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/TotalTripsbyUserType)
  * [Weekly Trips by User Type](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/WeeklyTripsbyUserType)
  * [Relevant Routes - From/To Stations](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/RelevantRoutes-FromToStations)
  * [Total Trips per Start Station](https://public.tableau.com/profile/matheus.gratz#!/vizhome/Citibike-GratzM_-TableauChallenge/TotalTripsperStartStation)

---
Matheus Gratz - [Github](https://github.com/matheusgratz/) - [Email](matheusgratz@gmail.com) - 2020