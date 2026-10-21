# Open-Meteo Data Engineering Pipeline

## Overview

This project implements an automated data engineering pipeline using the Open-Meteo REST API and Databricks Free Edition. It collects weather forecast data, transforms it through a Medallion architecture, and produces analytical tables for reporting.

The pipeline is orchestrated using a Databricks Job that executes the Bronze, Silver, and Gold notebooks in sequence.

## Objectives

- Extract weather forecast data from a REST API.
- Store raw API responses for traceability.
- Transform JSON data into structured hourly records.
- Generate daily weather metrics using PySpark.
- Automate notebook execution through Databricks Jobs.
- Prepare analytical datasets for dashboards and reporting.

## Technology Stack

- **Python:** API requests and JSON processing.
- **PySpark:** Data transformation and aggregation.
- **SQL:** Data validation and analytical queries.
- **Databricks Free Edition:** Development and job orchestration.
- **Delta Lake:** Structured data storage.
- **Open-Meteo API:** Weather forecast data source.
- **GitHub:** Project documentation and version control.

## Architecture

The project follows the Medallion architecture:

### Bronze — Raw Data

Retrieves weather forecast data from Open-Meteo and stores the original JSON response with extraction metadata in a Delta table.

Table: `bronze_weather_raw`

### Silver — Cleaned Data

Reads the most recent Bronze extraction, converts hourly API data into structured records, applies data type conversions, and removes duplicate timestamps.

Table: `silver_weather_hourly`

### Gold — Analytics

Aggregates hourly measurements into daily metrics, including average, minimum, and maximum temperature, average humidity, total precipitation, and average wind speed.

Table: `gold_weather_daily`

## Pipeline Orchestration

A Databricks Job named `OpenMeteo_ETL_Job` orchestrates the following tasks:

1. `bronze_ingestion`
2. `silver_transformation`
3. `gold_analytics`

Task dependencies enforce the execution order. Silver runs after Bronze succeeds, and Gold runs after Silver succeeds.


## Data Source

Open-Meteo Forecast API: https://open-meteo.com/en/docs

The project uses hourly weather variables for a location in San José, Costa Rica:

- Temperature at 2 meters
- Relative humidity at 2 meters
- Precipitation
- Wind speed at 10 meters

The forecast horizon is configured for seven days.

## How to Run

1. Open the Databricks workspace.
2. Configure the project catalog and schema.
3. Run the Bronze notebook to retrieve and store the API response.
4. Run the Silver notebook to transform the latest extraction.
5. Run the Gold notebook to calculate daily metrics.
6. Execute `OpenMeteo_ETL_Job` to run the complete pipeline.

The catalog name and schema must match the environment configuration.

## Current Implementation

- REST API ingestion.
- Bronze, Silver, and Gold Delta tables.
- Basic data cleaning and duplicate removal in Silver.
- Daily analytical aggregations in Gold.
- Sequential notebook orchestration using Databricks Jobs.

## Future Improvements

- Add automated data quality checks.
- Implement deterministic deduplication and incremental processing.
- Add error handling, logging, and monitoring.
- Expand the pipeline to multiple locations.
- Integrate historical weather data.
- Create an interactive analytics dashboard.

## Author

Moises Cubero Jimenez

Developed using Databricks Free Edition, Python, PySpark, SQL, Delta Lake, and Open-Meteo.