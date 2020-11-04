# Temperature-based_RT_project
Repo for the temperature-based retention time project

# Temperature based retention time: Project Overview 

* Created a tool that could determine the retention times of a wastewater parcel in a pressure pipe using temperature data
* Tested and calibrated the tool using historical data from a study conducted by the TU Berlin as well as field data from a real pressure pipe system in the town of Ueckermünde, Germany

<img align="left" width="430" height="350" src="https://github.com/moe221/Temperature-based_RT_project/blob/main/Images/Ueckerm%C3%BCnde/pumping%20events%20-%20one%20week.png">

<img align="right" width="400" height="350" src="https://github.com/moe221/Temperature-based_RT_project/blob/main/Images/Ueckerm%C3%BCnde/temp%20%26%20flow%20ART%20hourly.png">


## Code and Resources Used 
**Python Version:** 3.7  
**Packages:** pandas, numpy, sklearn, matplotlib, seaborn, scipy, plotly, statsmodels 

## Background and concept

Most sewer networks consist of gravity pipes, which rely on the natural slope of the landscape to convey wastewater, and pressure pipes, which are driven by a pump that pushes wastewater to higher located collection points. Anaerobic retention time (ART) is defined as the amount of time a given volume of wastewater resides in a pressure pipe before being discharged. Long retention times have been linked to an increase in the occurrence of unwanted biological and chemical processes, such as the formation of hydrogen sulfide (H2S) which is associated with odor, corrosion and health problems. Retention time is most commonly determined using flow rate data. 

This project aim to test and validate a new method for determining retention times using only temperature measurments. The method assumes that by continuously monitoring wastewater temperatures at the outlet of a pressure pie using online sensors, discharges can be detected indirectly and used to determine retention times. 

## Data source
The temperature-based ART calculation method, referred to as the “ART-temp” method in this notebook, was tested using historical data collected during a pilot scale experiment conducted by the TU Berlin (dataset A). Furthermore, the method was validated using filed data from a real pressure pipe systems in Germany (dataset b) to ensure validity and determine limitations and potential applications. Flow rate data and pumping operation times were used as a reference to the results generated by the proposed temperature method. 

The data cosisted of:

* liquid-phase Hydrogen sulfide (H2S) measutemnts [mg/L]
* Wastewater temperature [°C]
* Flow rate [m³/h]

Temperature and H2S measurements were collected using online SulfiloggerTM sensors. Flow rate measurments were collected using magnet-induced flow meters (MID). 


## Data Cleaning
Flow-meter and the SulfiloggerTM data was combined into one dataset with three columns and a datetime index. The dataset was then prepared for analysis by locating duplicates, measurement errors and missing values and correcting them when possible or removing them completely from the dataset. 

## EDA
In order to understand the data obtained from each dataset, a short analysis of the data was conducted to develop insight, locate trends and patterns and determine the overall behaviour of the systems.

<img align="right" width="350" height="300" src="https://github.com/moe221/Temperature-based_RT_project/blob/main/Images/Ueckerm%C3%BCnde/Boxplot%20H2S.png">

<img align="left" width="350" height="300" src="https://github.com/moe221/Temperature-based_RT_project/blob/main/Images/Ueckerm%C3%BCnde/Boxplot%20temp.png">


<img align="center" src="https://github.com/moe221/Temperature-based_RT_project/blob/main/Images/Ueckerm%C3%BCnde/Flow%20rate%20and%20temp%20peaks%20-%20one%20day.png">


## Feature engineering (Refernce retention times)
To calculate the refernce retention times, flow data was transformed in to pump operation time data.The times were the pump was active were determined by analysing flow data and locating the time intervals were the predetermined pumping flow rate was reached. the retention time was calculated using a function that tracks the accumulative volume of every parcel entering the pressure pipe with every pumping event using flow rate measurements at different time horizons. Below is an example of the generated pump operation times and the resulting retention times. 

## Temperature retention time algorithm

The temperature data was smoothed using the ewm function and local changes in temperature were detected using the argrelextrema() function. A dataset of pump operation times was generated based on the time stamps of the detected discharges. Figure 10 shows an example of smoothed temperature timeseries with the detected peaks and minima highlighted 

The algorithm then calculates the number of pumping events needed for a single parcel to pass through the pressure pipe

## Results 
Four different evaluation methods were used to compare the results:
*	**Probability analysis** – likelihood of temperature change to occure given that a pumping event has already occurred 
*	**t-test** – To see if temperature based results and reference results a significantlly different from each other.
*	**Linear Regression** – To see how well temperature based results can predict reference results.
* **Root Mean Sqaured Error (RMSE)** - To have an error metric when directlly comparing temperature based results to the reference results

**Dataset A results**
<p align="center">
  <img src="https://github.com/moe221/Temperature-based_RT_project/blob/main/Images/Pilot%20Plant/ART-flow%20%26%20temp%20-%20complete.png"/>
</p>

**Dataset B results**

<p align="center">
  <img src="https://github.com/moe221/Temperature-based_RT_project/blob/main/Images/Ueckerm%C3%BCnde/ART-flow%20%26%20temp%20-%20complete.png"/>
</p>


## Method performance
*	**Probability analysis** : 98% probability (Dataset A), 81% probability (Dataset B)
*	**t-test**: p- value = 0.92 (Dataset A) and 0.61 (Dataset B) --> no significant difference found
*	**Linear Regression**: R² = 0.97 ( Dataset A), and 0.75 (Dataset B)
* **Root Mean Sqaured Error (RMSE)**: RMSE = 11% (Dataset A), RMSE = 14% (Dataset B)

