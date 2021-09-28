# PyBer_Analysis

## Overview & Purpose
Looking at rideshare data from January to early May of 2019 the purpose of this assignment was to perform an analysis of ride-sharing data by city type (urban, suburban and rural.

This would be accomplished by crated a Pandas DataFrame summarizing the data and then creating a multiple-line graph using Matplotlib that describes total weekly fares for each city type.

## Analysis
This analysis consisted of two datasets: city data and ride data.

### Create a DataFrame of Ride Data by City Type
#### Each dataset had to be read in and then merged
```
# File to Load (Remember to change these)
city_data_to_load = "Resources/city_data.csv"
ride_data_to_load = "Resources/ride_data.csv"

# Read the City and Ride Data
city_data_df = pd.read_csv(city_data_to_load)
ride_data_df = pd.read_csv(ride_data_to_load)

# Combine the data into a single dataset
pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])
```

#### Using the merged DataFrame, created new series of required values
```
sum_rides_city_type = pyber_data_df.groupby(["type"]).count()["ride_id"]
sum_rides_city_type

unique_cities_df = pyber_data_df.drop_duplicates(subset = ["city"])
sum_drivers_city_type = unique_cities_df.groupby(["type"]).sum()["driver_count"]
sum_drivers_city_type

sum_fares_city_type = pyber_data_df.groupby(["type"]).sum()["fare"]

average_fare_city_type = sum_fares_city_type / sum_rides_city_type

average_driver_fare_city_type = sum_fares_city_type / sum_drivers_city_type
```

#### Create the Summary DataFrame
```
summary_df = pd.DataFrame(
    {"Total Rides": sum_rides_city_type,
                           "Total Drivers": sum_drivers_city_type,
                           "Total Fares": sum_fares_city_type,
                           "Average Fare per Ride": average_fare_city_type,
                           "Average Fare per Driver": average_driver_fare_city_type})
                           
summary_df["Total Rides"] = summary_df["Total Rides"].map("{:,}".format)
summary_df["Total Drivers"] = summary_df["Total Drivers"].map("{:,}".format)
summary_df["Total Fares"] = summary_df["Total Fares"].map("${:,.2f}".format)
summary_df["Average Fare per Ride"] = summary_df["Average Fare per Ride"].map("${:,.2f}".format)
summary_df["Average Fare per Driver"] = summary_df["Average Fare per Driver"].map("${:,.2f}".format)

summary_df
```
![DataFrame_Summary](https://user-images.githubusercontent.com/89284280/135006626-9f4b18b3-7347-413e-a715-5caa692ace32.PNG)


### Create Multiple-Line Graphy

