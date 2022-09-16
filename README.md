# Project
---
title: "Bike-Share"
author: "Anita Best"
date: "2022-06-28"
output:
  pdf_document: default
  html_document: default
---

# INSTALLING PACKAGES


```{r}
install.packages("tidyverse")
library(tidyverse)
```
```{r}
install.packages("skimr")
library(skimr)
```
```{r}
install.packages("janitor")
library(janitor)
```




# COLLECTING DATA AND COMPARING COLUMNS

```{r}
Trips_April21 <- read.csv("Downloads/Capstone 21-22/202104-divvy-tripdata.csv")
colnames(Trips_April21)
```

```{r}
Trips_May21 <- read.csv("Downloads/Capstone 21-22/202105-divvy-tripdata.csv")
colnames(Trips_May21)
```


```{r}
Trips_June21 <- read.csv("Downloads/Capstone 21-22/202106-divvy-tripdata.csv")
colnames(Trips_June21)
```


```{r}
Trips_July21 <- read.csv("Downloads/Capstone 21-22/202107-divvy-tripdata.csv")
colnames(Trips_July21)
```


```{r}
Trips_Aug21 <- read.csv("Downloads/Capstone 21-22/202108-divvy-tripdata.csv")
colnames(Trips_Aug21)
```


```{r}
Trips_Sept21 <- read.csv("Downloads/Capstone 21-22/202109-divvy-tripdata.csv")
colnames(Trips_Sept21)
```


```{r}
Trips_Oct21 <- read.csv("Downloads/Capstone 21-22/202110-divvy-tripdata.csv")
colnames(Trips_Oct21)
```


```{r}
Trips_Nov21 <- read.csv("Downloads/Capstone 21-22/202111-divvy-tripdata.csv")
colnames(Trips_Nov21)
```


```{r}
Trips_Dec21 <- read.csv("Downloads/Capstone 21-22/202112-divvy-tripdata.csv")
colnames(Trips_Dec21)
```


```{r}
Trips_Jan22 <- read.csv("Downloads/Capstone 21-22/202201-divvy-tripdata.csv")
colnames(Trips_Jan22)
```


```{r}
Trips_Feb22 <- read.csv("Downloads/Capstone 21-22/202202-divvy-tripdata.csv")
colnames(Trips_Feb22)
```


```{r}
Trips_March22 <- read.csv("Downloads/Capstone 21-22/202203-divvy-tripdata.csv")
colnames(Trips_March22)
```

# INSPECTING DATAFRAMES

```{r}
str(Trips_April21)
```

```{r}
str(Trips_May21)
```


```{r}
str(Trips_June21)
```


```{r}
str(Trips_July21)
```


```{r}
str(Trips_Aug21)
```


```{r}
str(Trips_Sept21)
```


```{r}
str(Trips_Oct21)
```


```{r}
str(Trips_Nov21)
```


```{r}
str(Trips_Dec21)
```


```{r}
str(Trips_Jan22)
```


```{r}
str(Trips_Feb22)
```


```{r}
str(Trips_March22)
```



#COMPARING DATA FRAMES

## Using the compare_df_cols function to see if dataframes can bind together successfully

```{r}
compare_df_cols(Trips_April21, Trips_May21, Trips_June21, Trips_July21, Trips_Aug21, 
                Trips_Sept21, Trips_Oct21, Trips_Nov21, Trips_Dec21, Trips_Jan22,
                Trips_Feb22, Trips_March22, return = "mismatch")
```

# BIND INDIVIDUAL DATA FRAMES (ROWS) INTO ONE DATA FRAME

```{r}
all_trips <- bind_rows(Trips_April21, Trips_May21, Trips_June21, Trips_July21, Trips_Aug21, 
                Trips_Sept21, Trips_Oct21, Trips_Nov21, Trips_Dec21, Trips_Jan22,
                Trips_Feb22, Trips_March22)
```

# REMOVE UNUSED COLUMN

```{r}
all_trips <- all_trips %>% 
  select(-c(start_lat, start_lng, end_lat, end_lng))
```

# RENAME COLUMNS

```{r}
all_trips <- all_trips %>% 
  rename(trip_id = ride_id, ride_type = rideable_type, start_time = started_at,
                        end_time = ended_at, from_station_name = start_station_name, from_station_id = start_station_id,
                        to_station_name = end_station_name, to_station_id = end_station_id, user_type = member_casual)
```


# CLEANING AND PREPARING DATA FOR ANALYSIS


## checking the first 6 rows of the data frame
```{r}
head(all_trips)
```


## list of columns and data types

```{r}
str(all_trips)
```


## summary
```{r}
summary(all_trips)
```


```{r}
skim(all_trips)
```


## Adding columns to include date: month, day and year of each ride

```{r}
all_trips$date <- as.Date(all_trips$start_time)
all_trips$month <- format(as.Date(all_trips$date), "%m")
all_trips$day <- format(as.Date(all_trips$date), "%d")
all_trips$year <- format(as.Date(all_trips$date), "y")
all_trips$day_of_week <- format(as.Date(all_trips$date), "%A")
```


## Add a "ride length" calculation to all trips (in seconds)

```{r}
all_trips$ride_length <- difftime(all_trips$end_time, all_trips$start_time)
```



## convert ride_length from factor to numeric to run calculations on the data

```{r}
is.factor(all_trips$ride_length)
```

```{r}
all_trips$ride_length <- as.numeric(as.character(all_trips$ride_length))
is.numeric(all_trips$ride_length)
```

# REMOVE BAD DATA
## over hundreds of data entries was negative where bikes were taken out of dock and checked for quality by divvy or ride_length


```{r}
skim(all_trips$ride_length)
```


```{r}
all_trips_v2 <- all_trips[!(all_trips$ride_length<0),]
skim(all_trips_v2)
```


# CONDUCT DESCRIPTIVE ANALYSIS

```{r}
summary(all_trips_v2$ride_length)
```

# EXPORT TO CSV FILE FOR FURTHER ANALYSIS

```{r}
write.csv(all_trips_v2, "data.csv")
```



