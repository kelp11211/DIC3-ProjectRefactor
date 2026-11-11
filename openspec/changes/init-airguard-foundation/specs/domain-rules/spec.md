# Spec Delta

## Purpose

Encapsulates Taiwan MOENV 2025 revised AQI classification thresholds, sub-indices, and activity guidance for general and sensitive populations.

## ADDED Requirements

### Requirement: 2025 Revised AQI Classification
The system SHALL evaluate AQI levels according to the Taiwan MOENV 2025 revised thresholds (Good 0-50, Moderate 51-100, Unhealthy for Sensitive Groups 101-150, Unhealthy 151-200, Very Unhealthy 201-300, Hazardous 301-500).

#### Scenario: Categorize AQI value
- **WHEN** official AQI value is 125
- **THEN** system assigns category `Unhealthy for Sensitive Groups` with legal guide version `MOENV_2025`

### Requirement: Population-Specific Activity Guidance
The system SHALL map evaluated AQI category and user population group (general vs sensitive) to official MOENV activity guidance without hallucinating medical recommendations.

#### Scenario: Sensitive group under orange alert
- **WHEN** AQI category is `Unhealthy for Sensitive Groups` and population group is `sensitive`
- **THEN** system outputs official advice to reduce prolonged outdoor strenuous exertion and cites official MOENV guideline URL

### Requirement: Strict Guardrail Against Medical Overclaim
The system SHALL refuse to provide personalized medical dosage, medication adjustment, or health outcome guarantees.

#### Scenario: User inquires medical medication
- **WHEN** user query contains medication dosage questions such as inhaler puffs
- **THEN** system refuses medical advice and outputs disclaimer redirecting to healthcare professionals
