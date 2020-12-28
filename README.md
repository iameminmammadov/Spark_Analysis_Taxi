<!-- PROJECT LOGO -->
<br />
<p align="center">
    <img src="images/logo_lhl.png" alt="Logo" width="500" height="200">
  </a>

  <h3 align="center">Taxi Analysis using Spark Project</h3>
</p>

## Table of Contents

* [About the Project](#about-the-project)
  * [Built using](#built-using)
* [Getting Started](#getting-started)
* [Contributing](#contributing)
* [License](#license)
* [Contact](#contact)


<!-- ABOUT THE PROJECT -->
## About The Project

This project has been created as part of my contract work with Lighthouse Labs: Canada's 
Leading Coding Bootcamp. This material has been created for a startup, located in Toronto, ON, as part of their effort to utilize various tools from machine learning/data science. This notebook among others would be used to onboard new members of a team.


### Built Using
Here the tools that you will learn (I will teach you) in order to understand how this project works:
* [GCP DataProc](https://console.cloud.google.com/dataproc)
* [GCP Storage](https://console.cloud.google.com/storage)
* [PySpark](https://pypi.org/project/pyspark/)



<!-- GETTING STARTED -->
## Getting Started with PySpark

Because we will be running PySpark on Google servers, there is no need to install or configure the 
Spark package. This source (https://medium.com/swlh/pyspark-on-macos-installation-and-use-31f84ca61400)
is a pretty good tutorial.

## Getting Started with GCP Storage

We need to get and store data for future analysis. In order to store data, we will create a bucket. However, the name of the bucket is not that important. To create a bucket, the following stepts must be taken:
* `Create a bucket` 
* Create a name 
* Skip other options
* `Create` button on the bottom of the list

Once you create the bucket, we will need to get data inside of that bucket. First, we start with the actual NYC Taxi data:
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-01.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-01.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-02.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-02.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-03.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-03.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-04.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-04.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-05.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-05.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-06.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-06.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-07.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-07.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-08.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-08.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-09.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-09.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-10.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-10.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-11.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-11.csv`
`!curl -L https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2019-12.csv | gsutil cp - gs://name_of_created_bucket/trip_data/yellow_tripdata_2019-12.csv`

In order to enhance our dataset, we will add data available as public datasets on GCP Big Query:
`!bq --location=US extract --compression GZIP 'bigquery-public-data:new_york_taxi_trips.tlc_yellow_trips_2019' gs://name_of_created_bucket/nyc_taxi_trips/nyc_taxi_2018-*.csv.gz`
`!bq --location=US extract --compression GZIP 'bigquery-public-data:new_york_taxi_trips.taxi_zone_geom' gs://name_of_created_bucket/nyc_taxi_trips/nyc_taxi_zones-*.csv.gz`
`!bq --location=US extract --compression GZIP 'bigquery-public-data:noaa_gsod.stations' gs://name_of_created_bucket/nyc_weather/noaa_stations-*.csv.gz`
`!bq --location=US extract --compression GZIP 'bigquery-public-data:noaa_gsod.gsod2019' gs://name_of_created_bucket/nyc_weather/noaa_weather-*.csv.gz`

## Getting Started with GCP Dataproc

The main reason behind using GCP Dataproc and not AWS EMR is mainly due to billing policies. GCP allows to create and play with Hadoop clusters for free, while AWS does not allow to do so, and charges incurred during the usage of EMR could skyrocket. 

To start with Dataproc, `Enable` API, and then we will need to create a cluster:
* `Create Cluster`
* Chose a name for `Cluster name`. Feel free to leave as default. For region, choose `us-central-1`, and for cluster type `Standard`.
* For Image version, choose `1.3 (Ubuntu 18.04 LTS, Hadoop 2.9, Spark 2.3).
* For `Components`, choose `Enable Component Gateway` as we will need to open Jupyter notebooks
* For optional components, choose `Jupyter Notebook` and `Anaconda`.
* In the setting `Customise Cluster`, scroll down to `Cloud Storage staging bucket`, `browse` and `Create new bucket`. You will need this bucket to store jupyter notebooks
* `Create`

After creating the cluster, click on it, and go to `Web Interfaces` and choose `Jupyter` option. Right after that, treat this as a general Jupyter notebook but remember to create a `PySpark` notebook when you start a new one.


__Note__: It can take some time to enable the API, create and start Hadoop clusters. Don't panic if nothing is happening for 5-10 minutes.

<!-- CONTRIBUTING -->
## Contributing

While this project has been created with the certain goal in mind, anyone from LightHouse Labs or anyone else can cantribute. Any contributions you make are **greatly appreciated** and will be **reviewed**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/Innovation`)
3. Commit your Changes (`git commit -m 'Add some Innovating feature that Emin didn't do'`)
4. Push to the Branch (`git push origin feature/Innovation`)
5. Open a Pull Request

After that, I will review it and either merge, or comment back. 

<!-- LICENSE -->
## License

Distributed under the MIT License. See [LICENSE](https://github.com/iameminmammadov/bigmart/blob/main/licence.txt) for more information.

<!-- CONTACT -->
## Contact

Emin Mammadov - [@LinkedIn](https://www.linkedin.com/in/emin-mammadov/) - emin.e.mammadov@gmail.com 

[Project Link](https://github.com/iameminmammadov/Spark_Analysis_Taxi)