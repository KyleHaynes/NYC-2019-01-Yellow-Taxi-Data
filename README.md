# NYC 2019-01 Yellow Taxi Data

NYC taxi data sourced from: https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page

A BZ2 (70meg) compressed file of the 2019-01: Yellow Taxi Trip Records CSV (655meg).

Repex code to download / inspect / compress file ...

```R
# load `data.table` and `R.utils`
# install.packages(c("data.table", "R.utils"))
library(data.table)
library(R.utils)

# Define url and file names
url = "https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-01.csv"
file_input = normalizePath("./file.csv", mustWork = F)
file_csv_output = normalizePath("./yellow_tripdata_2019-01.csv", mustWork = F)

# Download the data.
download.file(url, file_input)
    # trying URL 'https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-01.csv'
    # Content type 'text/csv' length 687088084 bytes (655.3 MB)
    # downloaded 655.3 MB

# Read data.
dt = fread(file_input)

# Inspect structure.
str(dt)
    # Classes ‘data.table’ and 'data.frame':  7667792 obs. of  18 variables:
    #  $ VendorID             : int  1 1 2 2 2 2 2 1 1 1 ...
    #  $ tpep_pickup_datetime : POSIXct, format: "2019-01-01 00:46:40" "2019-01-01 00:59:47" "2018-12-21 13:48:30" "2018-11-28 15:52:25" ...
    #  $ tpep_dropoff_datetime: POSIXct, format: "2019-01-01 00:53:20" "2019-01-01 01:18:59" "2018-12-21 13:52:40" "2018-11-28 15:55:45" ...
    #  $ passenger_count      : int  1 1 3 5 5 5 5 1 1 2 ...
    #  $ trip_distance        : num  1.5 2.6 0 0 0 0 0 1.3 3.7 2.1 ...
    #  $ RatecodeID           : int  1 1 1 1 2 1 2 1 1 1 ...
    #  $ store_and_fwd_flag   : chr  "N" "N" "N" "N" ...
    #  $ PULocationID         : int  151 239 236 193 193 193 193 163 229 141 ...
    #  $ DOLocationID         : int  239 246 236 193 193 193 193 229 7 234 ...
    #  $ payment_type         : int  1 1 1 2 2 2 2 1 1 1 ...
    #  $ fare_amount          : num  7 14 4.5 3.5 52 3.5 52 6.5 13.5 10 ...
    #  $ extra                : num  0.5 0.5 0.5 0.5 0 0.5 0 0.5 0.5 0.5 ...
    #  $ mta_tax              : num  0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 ...
    #  $ tip_amount           : num  1.65 1 0 0 0 0 0 1.25 3.7 1.7 ...
    #  $ tolls_amount         : num  0 0 0 0 0 5.76 0 0 0 0 ...
    #  $ improvement_surcharge: num  0.3 0.3 0.3 0.3 0.3 0.3 0.3 0.3 0.3 0.3 ...
    #  $ total_amount         : num  9.95 16.3 5.8 7.55 55.55 ...
    #  $ congestion_surcharge : num  NA NA NA NA NA NA NA NA NA NA ...
    #  - attr(*, ".internal.selfref")=<externalptr> 

# Write out.
fwrite(dt, file_csv_output)

# Report file size.
file.size(file_csv_output)
# 700318011

# bc2 compress file.
bzip2(file_csv_output)

# Report md5 checksum.
tools::md5sum(bz_path <- paste0(file_csv_output, ".bz2"))
# "c0dcb4c0e3bb0846a2364380623d741b"

# And file size.
file.size(bz_path)
# 72461916
```
