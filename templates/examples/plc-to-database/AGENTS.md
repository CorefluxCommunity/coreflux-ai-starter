# AGENTS.md

## Project Overview
This is a Coreflux LoT project for a Siemens S7 PLC data pipeline on production Line 1.
The system reads PLC tags (temperature, pressure) every 500ms, validates readings, triggers
alerts on threshold violations, and logs all data to PostgreSQL for historical analysis.

## Tech Stack
- **Platform:** Coreflux MQTT Broker
- **Language:** LoT (Language of Things)
- **Industrial Protocol:** Siemens S7 (S7-1500 at 192.168.1.10)
- **Database:** PostgreSQL (at db.local, database: production_db)
- **IDE:** VS Code with LoT Notebooks extension / Cursor

## LoT Naming Conventions
- **Actions:** PascalCase, verb-first (ProcessPLCData, CalculateOEE, MonitorThresholds)
- **Models:** PascalCase, noun-based (PLCReading, AlarmRecord, ProductionMetric)
- **Rules:** PascalCase, descriptive scope (ProtectPLCTopics, ProtectSysTopics)
- **Routes:** PascalCase, destination-based (S7Line1, ProductionLog, AlarmHistory)
- **Variables:** snake_case in quotes ("tag_value", "cycle_time", "equipment_id")
- **Topics:** lowercase/slash-separated (plc/line1/temperature, alerts/critical/+)
- **Model fields:** snake_case ("tag_name", "raw_value", "engineering_unit")

## Topic Hierarchy
- plc/line1/          — Raw PLC tag values from S7 Route (plc/line1/temperature, plc/line1/pressure)
- processed/line1/    — Validated and enriched data
- alerts/critical/    — Safety-critical alarms (temperature > 80°C, pressure > 150 bar)
- alerts/warning/     — Non-critical warnings (approaching thresholds)
- production/line1/   — Production metrics (OEE, cycle times, batch data)
- config/             — Configurable setpoints and thresholds
- state/line1/        — Internal persistent state (cycle counters, last values)
- system/             — Heartbeats and diagnostics

## Industrial Route Conventions
- PLC Route name reflects the physical line/area: S7Line1
- Polling interval: 500ms (control-level monitoring)
- All PLC tags specify DATA_TYPE explicitly (REAL, INT, BOOL)
- SOURCE_TOPIC pattern: plc/[line]/[tag_name]
- Include UNIT and DECIMAL_PLACES for engineering clarity

## Database Route Conventions
- Database Route name reflects purpose: ProductionLog, AlarmHistory
- Every INSERT includes a timestamp column (NOW())
- One EVENT per source topic — do not overload
- Table structure: sensor_data(timestamp, tag_name, value, unit)

## Project-Specific Rules
- Temperature alert threshold: 80°C (critical), 70°C (warning)
- Pressure alert threshold: 150 bar (critical), 130 bar (warning)
- PLC address map: DB1.DBD100 = temperature (REAL), DB1.DBD104 = pressure (REAL)
- All PLC topics are read-only from the broker side — never write to PLC tags without confirmation
- Alarm payloads must include: equipment_id, tag_name, value, threshold, timestamp

## Code Standards
- Always type-cast in calculations: PAYLOAD AS DOUBLE, GET TOPIC ... AS DOUBLE
- Use KEEP TOPIC for internal state, PUBLISH TOPIC for external broadcast
- Mark the primary data field AS TRIGGER in models — never timestamps
- Use COLLAPSED models for alarm records (Action controls publish timing)
- Break complex Actions into callable Actions with INPUT/OUTPUT

## Do
- Always respect the indentation of 4 spaces per additional level
- Use PascalCase for all entity names
- Use snake_case for all variables and model fields
- Add type casts to every numeric operation
- Use lot as the code block language identifier
- Booleans and integers do not use quotes
- Consult the Coreflux MCP for documentation when unsure
- Routes that call functions from other protocols (like SQL queries) support the full range of that external element's capabilities
- Include engineering units in all published sensor values

## Don't
- Do not invent LoT syntax — only use documented keywords
- Do not use LOT (all caps) or lot (lowercase) — always LoT
- Do not trigger models on timestamp fields
- Do not use PUBLISH for internal-only state
- Do not create monolithic Actions — split into callables
- Do not poll the PLC faster than 500ms unless explicitly confirmed
- Do not hardcode PLC IP addresses in multiple places — define once in the Route

## Ask First
- Before adding or modifying Routes (PLC connections, database connections)
- Before modifying Rules (access control changes)
- Before removing any existing Action, Model, Rule, or Route
- Before changing PLC polling intervals (impacts controller load)
- Before modifying alarm thresholds (may have safety implications)
- Before using Python — confirm LoT can't handle it natively first

## Never
- Never write to PLC tags without explicit user confirmation
- Never hardcode credentials in Route definitions for production
- Never leave $SYS/# topics unrestricted
- Never skip type casting in mathematical operations
- Never generate LoT syntax you haven't verified in the documentation
- Never disable or lower priority of safety-related Rules
