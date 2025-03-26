# build_geofences
Python based Data Pipeline that reads Microsoft Buildings dataset to create building geofences

This repository contains a Python-based geofencing pipeline that processes building footprint data to generate geofence polygons. The script leverages Dask for parallel computation, Shapely for geometric operations, and Snowflake for data storage and updates.

## Overview
The Geofence Pipeline performs the following tasks:

## Main script that implements the geofencing pipeline:

1. Extracts and processes location data.
2. Calculates geofence polygons with dynamic offsets.
3. Uploads data to Snowflake and updates location databases.

## Author
mpolisetti

## Data Extraction:
Queries location data from a Snowflake view to retrieve records in the source table that don't have geofence information.

## Geofence Calculation:
Processes location records by converting geometry strings to Shapely Polygons, computing the area of each building footprint, and applying a dynamic offset (based on the calculated area) to generate a buffered geofence polygon.
This pipeline uses Dask to efficiently parallelize these operations on large datasets.

## Snowflake Integration:
Uploads the resulting geofence data (including footprint IDs, geofence polygons, and area measurements) to Snowflake via temporary staging tables. The script then updates the main location tables:

It updates records in destination LOCATION table for rows with missing footprint IDs.

It inserts new geofence data into the LOCATION_GEOFENCE_1 table.

## Warehouse Management:
Dynamically adjusts the size of the Snowflake warehouse (scaling up during heavy processing and scaling down afterward) to optimize performance and cost.

## Features
Scalable Data Processing:
Uses Dask to convert Pandas DataFrames into distributed dataframes for handling large datasets efficiently.

## Geometric Computations:
Leverages Shapely and PyProj to calculate areas and apply spatial buffers, ensuring that geofence polygons are generated with appropriate offsets based on building size.

## Flexible Execution Modes:
A testFlag parameter allows switching between development (DEV schema) and production (PROD schema) modes.

## Robust Snowflake Integration:
Utilizes SQLAlchemy and Snowflake’s Python connector to manage temporary staging, bulk uploads, and database updates seamlessly.

## Getting Started
### Prerequisites
Python 3.x

Required Python packages:

pandas

dask

shapely

pyproj

sqlalchemy

snowflake-sqlalchemy

Install dependencies using pip:

bash
Copy
pip install pandas dask shapely pyproj sqlalchemy snowflake-sqlalchemy
Configuration

Ensure that the following environment variables are set for Snowflake authentication:

SNOWFLAKE_USER

SNOWFLAKE_PWD

Running the Pipeline
To execute the pipeline:

Clone the repository.

Run the script:

bash
Copy
python geofence_1_dask.py
By default, the script runs in production mode. To test in development mode, modify the testFlag variable accordingly in the main section.

File Structure
geofence_1_dask.py

This README provides an overview of the project, explains its key components, and guides users on how to set up and run the pipeline. Feel free to adjust details to better match your specific project context.
