# AGENTS.md

## Project Overview
This is a Coreflux LoT project for monitoring factory temperature sensors.
The system reads temperature data from multiple sensors, converts Celsius to Fahrenheit,
triggers alerts when thresholds are exceeded, and stores all readings in PostgreSQL.

## Tech Stack
- **Platform:** Coreflux MQTT Broker
- **Language:** LoT (Language of Things)
- **Database:** PostgreSQL (for historical sensor data)
- **IDE:** VS Code with LoT Notebooks extension / Cursor

## LoT Naming Conventions
- **Actions:** PascalCase, verb-first (ProcessTemperature, MonitorPressure)
- **Models:** PascalCase, noun-based (TemperatureReading, SensorStatus)
- **Rules:** PascalCase, descriptive scope (ProtectSysTopics, AllowSensorPublish)
- **Routes:** PascalCase, destination-based (SensorDatabase, AlertEmail)
- **Variables:** snake_case in quotes ("sensor_id", "raw_value", "temp_f")
- **Topics:** lowercase/slash-separated (sensors/+/temperature, alerts/temperature/+)
- **Model fields:** snake_case ("sensor_id", "value", "unit", "timestamp")

## Topic Hierarchy
- sensors/              — Raw incoming sensor data (sensors/+/temperature)
- processed/            — Converted and validated data (processed/+/celsius, processed/+/fahrenheit)
- alerts/temperature/   — Temperature threshold violations
- config/               — Configurable setpoints (config/max_temperature)
- state/                — Internal persistent state (state/last_alert)
- system/               — Heartbeats and diagnostics

## Project-Specific Rules
- Temperature alert threshold: 75°C
- All temperature values are in Celsius at the sensor level
- Fahrenheit conversion is handled by a callable Action (reusable)
- Models use the raw sensor value (DOUBLE) as the trigger field
- All readings are stored in a PostgreSQL table: temperature_readings(timestamp, sensor_id, value)

## Code Standards
- Always type-cast in calculations: PAYLOAD AS DOUBLE, GET TOPIC ... AS DOUBLE
- Use KEEP TOPIC for internal state, PUBLISH TOPIC for external broadcast
- Mark the primary data field AS TRIGGER in models — never timestamps
- Break complex Actions into callable Actions with INPUT/OUTPUT

## Do
- Always respect the indentation of 4 spaces per additional level
- Use PascalCase for all entity names
- Use snake_case for all variables and model fields
- Add type casts to every numeric operation
- Use lot as the code block language identifier
- Booleans and integers do not use quotes
- Consult the Coreflux MCP for documentation when unsure

## Don't
- Do not invent LoT syntax — only use documented keywords
- Do not use LOT (all caps) or lot (lowercase) — always LoT
- Do not trigger models on timestamp fields
- Do not use PUBLISH for internal-only state
- Do not create monolithic Actions — split into callables

## Ask First
- Before adding or modifying Routes (database connections)
- Before changing the alert threshold (currently 75°C)
- Before removing any existing Action, Model, or Route

## Never
- Never hardcode database credentials in Route definitions
- Never leave $SYS/# topics unrestricted
- Never skip type casting in mathematical operations
- Never generate LoT syntax you haven't verified in the documentation
