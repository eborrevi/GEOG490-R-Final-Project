US January Temperature Analysis 2000-2020
================
Eli Borrevik,
March 14, 2024

# Introduction cat

This document provides a visual analysis of yearly average temperatures,
linear regression to examine trends, and visualizations including
temperature maps.

# Load Necessary Libraries

Ensure all required libraries are loaded.

``` r
if (!requireNamespace("raster", quietly = TRUE)) install.packages("raster")
if (!requireNamespace("ggplot2", quietly = TRUE)) install.packages("ggplot2")

install.packages("raster")
install.packages("sp")
install.packages(“ggplot2”)
library(raster)
library(sp)
library(ggplot2)
```

# Calculate Yearly Average Temperature

Processing data to derive the average temperature for each year.

``` r
# Setting Base Path and Years Range
base_path <- "file_path"
years <- 2000:2020

# Initializing Vector for Yearly Averages
yearly_averages <- setNames(numeric(length(years)), years)

# Calculate Yearly Averages
for (year in years) {
  # Constructing File Name Pattern
  year_suffix <- sprintf("%02d", year - 2000)
  pattern <- sprintf("tmax_%s\\.bil$", year_suffix)
  year_files <- list.files(base_path, pattern = pattern, full.names = TRUE)
  
  if (length(year_files) > 0) {
    # Reading and Stacking Raster Files
    rasters <- lapply(year_files, raster)
    raster_stack <- stack(rasters)
    yearly_average_raster <- calc(raster_stack, mean)
    yearly_averages[as.character(year)] <- mean(values(yearly_average_raster), na.rm = TRUE)
  } else {
    yearly_averages[as.character(year)] <- NA
  }
}
```

# Visualization of Average Temperature by Year

![](Desktop/GIS_490/Deliverables/FIXED_AGAIN_JAN_LC.png)

Creating a plot to visually represent the temperature changes over the
years.

``` r
#plot Yearly Average Temperatures
yearly_data <- data.frame(Year = as.numeric(names(yearly_averages)),
                          AverageTemperature = yearly_averages)

ggplot(yearly_data, aes(x = Year, y = AverageTemperature)) +
  geom_line() + geom_point() + theme_classic() +
  scale_y_continuous(limits = c(0, 15)) +
  labs(title = "Yearly Average Temperature", x = "Year", y = "Average Temperature")
```

# 

# Regression Line on Average Temperatures

# ![](Desktop/GIS_490/Deliverables/TMAX_JAN_LR)

    #Linear regression
    model <- lm(AverageTemperature ~ Year, data = yearly_data)
    summary(model)
    #Regression line plotted on yearly average temperatures
    ggplot(yearly_data, aes(x = Year, y = AverageTemperature)) +
      geom_point() +
      geom_smooth(method = "lm", color = "red") +
      theme_minimal() +
      labs(title = "Yearly Average Temperature with Regression Line", 
           x = "Year", y = "Average Temperature")

# 

# Ordinary Least Square Analysis

    model <- lm(AverageTemperature ~ Year, data = yearly_data)
    summary(model)

January OLS ![](Desktop/GIS_490/Deliverables/TMAX_JAN_OLS.png)

July OLS![](Desktop/GIS_490/Deliverables/TMAX_JUL_OLS.png)

# Raster Map for January & July 2000

![](Desktop/GIS_490/Deliverables/TMAX_JAN_2000.png)
![](Desktop/GIS_490/Deliverables/TMAX_JUL_2000.png)

    file_path \<- paste0(base_path, "tmax_00.bil") raster_data \<- raster(file_path) plot(raster_data, main = "Temperature Map for January 2000", col = terrain.colors(100)) ''' \# ![]()![](Desktop/GIS_490/Deliverables/TMAX_JAN_2000.png)

![]()
