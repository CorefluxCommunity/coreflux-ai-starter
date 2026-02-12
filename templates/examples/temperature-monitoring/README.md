# Example: Temperature Monitoring

A basic sensor monitoring project that reads temperature data from MQTT topics, converts units, triggers alerts on threshold violations, and structures readings into consistent JSON.

## What This Demonstrates

- A filled-in `AGENTS.md` tailored to a specific use case (not the generic template)
- Topic hierarchy customized for sensor monitoring
- Project-specific thresholds and alarm rules

## System Overview

| Component | LoT Building Block | Purpose |
|-----------|--------------------|---------|
| ProcessTemperature | Action | Reads sensor data, checks thresholds, publishes alerts |
| ConvertToFahrenheit | Callable Action | Reusable unit conversion |
| TemperatureReading | Model | Structures raw data into consistent JSON |
| SensorDatabase | Route (PostgreSQL) | Stores readings for historical analysis |

## Topic Structure

```
sensors/+/temperature     — Raw sensor readings (trigger)
processed/+/celsius       — Validated readings
processed/+/fahrenheit    — Converted readings
alerts/temperature/+      — Threshold violations
state/last_alert          — Last alert timestamp (internal)
```

## How to Use

1. Copy the `AGENTS.md` from this folder into your project root
2. Set up the [MCP connection](https://docs.coreflux.org/ai/mcp#setup) in your editor
3. Start prompting your AI assistant to build each component

### Suggested prompt sequence:

```
1. "Create a LoT Action called ProcessTemperature that monitors sensors/+/temperature,
    extracts the sensor ID, and alerts when temperature exceeds 75°C"

2. "Create a callable Action ConvertToFahrenheit that takes celsius as input
    and returns fahrenheit"

3. "Create a Model TemperatureReading that structures sensor data as JSON
    with sensor_id, value, unit, and timestamp"

4. "Create a PostgreSQL Route to store temperature readings"
```

## Documentation

- [Using AI with Coreflux](https://docs.coreflux.org/quick-start/using-ai)
- [Developing with LoT Using AI](https://docs.coreflux.org/ai/developing-with-lot)
