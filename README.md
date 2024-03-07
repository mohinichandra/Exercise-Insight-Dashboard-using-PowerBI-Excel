# Exercise-Insight-Dashboard-using-PowerBI-Excel
A Power BI dashboard which lets users get an easy and quick overview of their exercise data.


### Data Model
Below is a screenshot of the data model after cleansed and prepared tables were read into Power BI.

We can see that the FACT table is connected to two dimension tables with the correct relationship established (1 to *) between dimension and fact tables.

![model](https://github.com/micky-26/Exercise-Insight-Dashboard-using-PowerBI-Excel/assets/106061980/b79cbf1c-21f2-49ab-883b-31ea79b602e2)


### Exercise Insight Dashboard
The finish report consists of two different dashboards. One is more of a basic version, while the second version contains more advanced visualizations. To enable these visualizations the calculation language DAX (Data Analysis Expressions) were used.

### Calculations: 

The following calculations were created in the Power BI reports using DAX (Data Analysis Expressions). To lessen the extent of coding, the re-use of measures (measure branching) was emphasized:

## Average Steps – This is a simple AVERAGE function around the Steps column:

``AVERAGE( FACT_Activity[Steps] )``

## Total Steps – This is a simple SUM function around the Steps column:

``SUM( FACT_Activity[Steps] )``

## Steps (Running) – This is a calculation to isolate the Total Steps measure by filtering it by the “Running Activity”:

``CALCULATE(
[Total Steps],
DIM_Activity[ActivityName] = “Running”
)
``

## Steps (Walking) – This is a calculation to isolate the Total Steps measure by filtering it by the “Walking Activity”:

``CALCULATE(
[Total Steps],
DIM_Activity[ActivityName] = “Walking”
)``

## Running % of Total – Here we are using two measures from before to find the % of steps that were done by running:

``DIVIDE(
[Steps (Running)],
[Total Steps]
)``

## Walking % of Total – Here we are using two measures from before to find the % of steps that were done by walking:

``DIVIDE(
[Steps (Walking)],
[Total Steps]
)``

## Total Steps (Cumulative) – Here we are re-using the Total Steps measure and using different functions to cumulatively calculate the total steps:

``CALCULATE(
[Total Steps],
FILTER(
ALLSELECTED( DIM_Date ),
DIM_Date[Date]
<= MAX( FACT_Activity[Date] )
)
)
``

## Week Over Week % Change Steps – Here we are using the Total Steps measure and using different functions, with variables, to calculate the Week over Week % Change of Steps:

``VAR CurrentWeek =
CALCULATE(
[Total Steps],
FILTER(
ALL( DIM_Date ),
DIM_Date[Week of Year]
= SELECTEDVALUE( DIM_Date[Week of Year] )
)
)
VAR PreviousWeek =
CALCULATE(
[Total Steps],
FILTER(
ALL( DIM_Date ),
DIM_Date[Week of Year]
= SELECTEDVALUE( DIM_Date[Week of Year] ) – 1
)
)
RETURN
DIVIDE(
( CurrentWeek – PreviousWeek ),
PreviousWeek
)
``
Basic Version: 

![Basic_Version](https://github.com/micky-26/Exercise-Insight-Dashboard-using-PowerBI-Excel/assets/106061980/2634323e-fd88-4c3b-a36c-1f75a92c719d)

Advanced Version: 

![Dashboard](https://github.com/micky-26/Exercise-Insight-Dashboard-using-PowerBI-Excel/assets/106061980/4b89da2b-b447-4b00-a821-1c6df58fef89)
