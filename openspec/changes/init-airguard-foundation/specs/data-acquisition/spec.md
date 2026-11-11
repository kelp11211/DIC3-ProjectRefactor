# Spec Delta

## Purpose

Provides verified retrieval, local caching, and data quality validation for Taiwan MOENV open air quality datasets AQX_P_432 and AQX_P_13.

## ADDED Requirements

### Requirement: Fetch Real-time AQI Dataset
The system SHALL retrieve hourly AQI records from MOENV dataset `AQX_P_432` with automatic retry budget and exponential backoff.

#### Scenario: Successful AQI retrieval
- **WHEN** client requests latest AQI for a monitored station
- **THEN** system fetches the dataset from MOENV API or warm cache and returns structured records with HTTP 200

#### Scenario: Upstream API failure with cache fallback
- **WHEN** MOENV API returns 500 error or connection times out
- **THEN** system serves cached record if within TTL and marks freshness status as stale with warning

### Requirement: Fetch Hourly PM2.5 Concentration
The system SHALL retrieve historical and recent hourly pollutant observations from MOENV dataset `AQX_P_13` and pivot monitor values into chronological time-series.

#### Scenario: Chronological time series transformation
- **WHEN** raw hourly table with MonitorValue00 to MonitorValue23 is ingested
- **THEN** system transforms values into long-format records ordered by UTC timestamp with verified unit in micrograms per cubic meter

### Requirement: Observation Data Quality Check
The system SHALL validate retrieved air quality records against schema rules, reject impossible values, and flag missing periods.

#### Scenario: Missing or out of bound values
- **WHEN** PM2.5 concentration value is negative or exceeds 1000 micrograms per cubic meter
- **THEN** system flags the observation as invalid and records rejection reason in quality report
