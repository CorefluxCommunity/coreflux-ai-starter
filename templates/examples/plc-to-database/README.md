# Example: PLC to Database Pipeline

An industrial data pipeline project that reads tag values from a Siemens S7 PLC, publishes them to MQTT topics, and stores all readings in a PostgreSQL database for historical analysis and dashboarding.

## What This Demonstrates

- A filled-in `AGENTS.md` tailored to an industrial PLC integration use case
- Industrial-specific naming conventions for Routes, tags, and topics
- OT-aware topic hierarchy separating PLC data from processed/alert topics
- Database Route conventions for structured data logging

## System Overview

| Component | LoT Building Block | Purpose |
|-----------|--------------------|---------|
| S7Line1 | Route (Siemens S7) | Reads PLC tags every 500ms, publishes to MQTT |
| ProcessPLCData | Action | Validates readings, applies thresholds, triggers alerts |
| PLCReading | Model | Structures PLC data into consistent JSON |
| ProductionLog | Route (PostgreSQL) | Stores all readings for historical queries |
| ProtectPLCTopics | Rule | Restricts write access to PLC command topics |

## Topic Structure

```
plc/line1/temperature     — Raw PLC tag value (from S7 Route)
plc/line1/pressure        — Raw PLC tag value (from S7 Route)
processed/line1/combined  — Validated and enriched data
alerts/critical/line1     — Safety-critical threshold violations
alerts/warning/line1      — Non-critical warnings
production/line1/metrics  — OEE and production KPIs
state/line1/last_cycle    — Internal state (cycle tracking)
```

## How to Use

1. Copy the `AGENTS.md` from this folder into your project root
2. Set up the [MCP connection](https://docs.coreflux.org/ai/mcp#setup) in your editor
3. Start prompting your AI assistant to build each component

### Suggested prompt sequence:

```
1. "Create a Siemens S7 Route called S7Line1 that reads temperature (REAL at DB1.DBD100)
    and pressure (REAL at DB1.DBD104) from a PLC at 192.168.1.10 every 500ms.
    Publish to plc/line1/temperature and plc/line1/pressure."

2. "Create an Action ProcessPLCData that monitors plc/line1/+ topics, checks if temperature
    exceeds 80°C or pressure exceeds 150 bar, and publishes alerts to alerts/critical/line1."

3. "Create a PostgreSQL Route called ProductionLog that stores all plc/line1/+ readings
    in a sensor_data table with columns: timestamp, tag_name, value."

4. "Create a Rule ProtectPLCTopics that denies all publish access to plc/# except
    for system clients."
```

## Documentation

- [Using AI with Coreflux](https://docs.coreflux.org/quick-start/using-ai)
- [Developing with LoT Using AI](https://docs.coreflux.org/ai/developing-with-lot)
- [Siemens S7 Routes](https://docs.coreflux.org/lot-language/routes/industrial/siemens-s7)
- [PostgreSQL Routes](https://docs.coreflux.org/lot-language/routes/data-storage/postgresql)
