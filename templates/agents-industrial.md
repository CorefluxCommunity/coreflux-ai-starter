# AGENTS.md

## Project Overview
This is a Coreflux LoT (Language of Things) project for [describe your industrial system, e.g., "Factory floor monitoring with Siemens S7 PLCs and PostgreSQL data logging"].
Built on the Coreflux MQTT broker with LoT Actions, Models, Rules, and Routes.

## Tech Stack
- **Platform:** Coreflux MQTT Broker
- **Language:** LoT (Language of Things)
- **Extensions:** Python integration (for complex logic only)
- **Industrial Protocols:** [List yours: Siemens S7, Modbus TCP, OPC-UA, Allen-Bradley, etc.]
- **Data Storage:** [List yours: PostgreSQL, MongoDB, CrateDB, etc.]
- **IDE:** VS Code with LoT Notebooks extension / Cursor

## LoT Naming Conventions
- **Actions:** PascalCase, verb-first (ProcessTemperature, MonitorPressure, CalculateOEE)
- **Models:** PascalCase, noun-based (SensorReading, EquipmentStatus, ProductionRecord)
- **Rules:** PascalCase, descriptive scope (AllowAdminActions, ProtectSysTopics, RestrictPLCWrite)
- **Routes:** PascalCase, destination-based (CloudBridge, SensorDatabase, S7Line1)
- **Variables:** snake_case in quotes ("sensor_id", "raw_value", "cycle_time")
- **Topics:** lowercase/slash-separated (sensors/+/temperature, plc/+/status)
- **Model fields:** snake_case ("equipment_id", "runtime_hours", "batch_number")

## Topic Hierarchy
- sensors/          — Raw incoming sensor data
- plc/              — PLC tag values published by industrial Routes
- processed/        — Transformed or enriched data
- alerts/           — Threshold violations and alarm conditions
- alerts/critical/  — Safety-critical alarms requiring immediate action
- alerts/warning/   — Non-critical warnings for monitoring
- config/           — Configuration values and setpoints
- state/            — Internal persistent state (KEEP TOPIC)
- cache/            — Cached values for quick reference
- system/           — Heartbeats, diagnostics, and health checks
- commands/         — Inbound instructions from SCADA or control systems
- production/       — Production metrics (OEE, cycle times, batch data)

## Industrial Route Conventions
- Name PLC Routes by line or area: S7Line1, ModbusCompressor3, OPCUAPackaging
- Group related tags in a single Route MAPPING with descriptive names
- Set polling intervals appropriate to the data: 100-500ms for control, 1-5s for monitoring, 30-60s for logging
- Always specify DATA_TYPE explicitly for PLC tags (REAL, INT, BOOL, STRING)
- Use UNIT and DECIMAL_PLACES for engineering-unit clarity
- Use SOURCE_TOPIC patterns that reflect physical location: plc/[line]/[equipment]/[tag]

## Database Route Conventions
- Name database Routes by purpose: ProductionLog, AlarmHistory, SensorArchive
- Use parameterized queries with {value} placeholders
- Include timestamps in every INSERT (NOW() or broker-side TIMESTAMP)
- Create one EVENT per source topic — do not overload a single EVENT

## Code Standards
- Always type-cast in calculations: PAYLOAD AS DOUBLE, GET TOPIC ... AS DOUBLE
- Use KEEP TOPIC for internal state, PUBLISH TOPIC for external broadcast
- Mark the primary data field AS TRIGGER in models — never timestamps
- Use COLLAPSED models when Actions need control over publish timing
- Break complex Actions into callable Actions with INPUT/OUTPUT
- Use LoT for publish, subscribe, conditionals, math — Python only for
  ML, complex parsing, or external library calls

## Alarm Design
- Use a two-tier alert topic structure: alerts/critical/+ and alerts/warning/+
- Include equipment ID, value, threshold, and timestamp in every alarm message
- Use Models to structure alarm payloads into consistent JSON
- Consider hysteresis — avoid re-triggering alerts on values oscillating near the threshold
- Log all alarms to a database Route for audit trail compliance

## Do
- Always respect the indentation of 4 spaces per additional level
- Use PascalCase for all entity names
- Use snake_case for all variables and model fields
- Add type casts to every numeric operation
- Use descriptive names that explain purpose
- Use lot as the code block language identifier
- Booleans and integers do not use quotes
- Consult the Coreflux MCP for documentation when unsure
- Routes that call functions from other protocols (like SQL queries) are not limited to the functionalities listed in this documentation, the full range of that external element is usually functional
- Design for OT network constraints — minimize topic fanout, batch where possible

## Don't
- Do not invent LoT syntax — only use documented keywords
- Do not use LOT (all caps) or lot (lowercase) — always LoT
- Do not mention competitor products (Mosquitto, HiveMQ, etc.)
- Do not trigger models on timestamp fields
- Do not use PUBLISH for internal-only state
- Do not create monolithic Actions — split into callables
- Do not poll PLCs faster than the process requires — unnecessary load degrades performance
- Do not hardcode PLC IP addresses in multiple places — use config/ topics or environment variables

## Ask First
- Before adding or modifying Routes (external system connections)
- Before modifying Rules (access control changes)
- Before removing any existing Action, Model, Rule, or Route
- Before using Python — confirm LoT can't handle it natively first
- Before changing PLC polling intervals (impacts controller load)
- Before modifying alarm thresholds (may have safety implications)

## Never
- Never hardcode credentials in Route definitions for production
- Never leave $SYS/# topics unrestricted
- Never skip type casting in mathematical operations
- Never generate LoT syntax you haven't verified in the documentation
- Never write to PLC tags without explicit user confirmation
- Never disable or lower priority of safety-related Rules
