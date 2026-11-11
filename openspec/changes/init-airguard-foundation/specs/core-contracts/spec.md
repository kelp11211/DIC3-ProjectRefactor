# Spec Delta

## Purpose

Defines strongly-typed data contracts and validation schemas across all AirGuard AI modules to ensure spatiotemporal consistency and provenance tracking.

## ADDED Requirements

### Requirement: Standardized Query Specification
The system SHALL parse user intent into a standardized `QuerySpec` containing activity type, station identifier, temporal window, population sensitivity group, and timezone.

#### Scenario: Valid query parsed
- **WHEN** user submits an inquiry for running at Taichung Situn station tomorrow morning
- **THEN** system produces a `QuerySpec` containing normalized site ID, UTC timestamp range, Asia/Taipei local time, and population group

### Requirement: Standardized Observation and AQI Entities
The system SHALL encapsulate official station readings into `Observation` and `OfficialAQI` models containing observation timestamp, publish timestamp, site identifier, value, unit, and source reference.

#### Scenario: Real-time AQI fetched
- **WHEN** official AQI record is retrieved from MOENV API
- **THEN** system validates that AQI integer, site name, published timestamp, and source URL are fully populated without null placeholders

### Requirement: Standardized Forecast and Decision Envelopes
The system SHALL encapsulate ML forecast results into `ForecastBundle` and agent response into `ResponseEnvelope` with explicit error bands and audit traces.

#### Scenario: Response envelope generation
- **WHEN** the agent finishes evaluating activity recommendations
- **THEN** system generates a `ResponseEnvelope` containing official AQI, forecast series, decision status, source citations, and limitations
