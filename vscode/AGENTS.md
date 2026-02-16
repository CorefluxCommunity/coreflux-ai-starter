# AGENTS.md

## Project Overview
This is a Coreflux LoT (Language of Things) project for [describe your system].
Built on the Coreflux MQTT broker with LoT Actions, Models, Rules, and Routes.

## Tech Stack
- **Platform:** Coreflux MQTT Broker
- **Language:** LoT (Language of Things)
- **Extensions:** Python integration (for complex logic only)
- **IDE:** VS Code with LoT Notebooks extension / Cursor

## LoT Naming Conventions
- **Actions:** PascalCase, verb-first (ProcessTemperature, MonitorPressure)
- **Models:** PascalCase, noun-based (SensorReading, EquipmentStatus)
- **Rules:** PascalCase, descriptive scope (AllowAdminActions, ProtectSysTopics)
- **Routes:** PascalCase, destination-based (CloudBridge, SensorDatabase)
- **Variables:** snake_case in quotes ("sensor_id", "raw_value")
- **Topics:** lowercase/slash-separated (sensors/+/temperature, alerts/critical/+)
- **Model fields:** snake_case ("equipment_id", "runtime_hours")

## Topic Hierarchy
- sensors/    — Raw incoming data
- processed/  — Transformed data
- alerts/     — Threshold violations
- config/     — Configuration values
- state/      — Internal persistent state (KEEP TOPIC)
- cache/      — Cached values
- system/     — Heartbeats and diagnostics
- commands/   — Inbound instructions

## Code Standards
- Always type-cast properly: PAYLOAD AS DOUBLE, (PAYLOAD AS DOUBLE), GET TOPIC ... AS <type>
- Use KEEP TOPIC for internal state, PUBLISH TOPIC for external broadcast
- Mark the primary data field AS TRIGGER in models — never timestamps
- Use COLLAPSED models when Actions need control over publish timing
- Break complex Actions into callable Actions with INPUT/OUTPUT
- Use LoT for publish, subscribe, conditionals, math — Python only for
  ML, complex parsing, or external library calls

## Do
- Whenever there is a question regarding IIoT, Coreflux and Language-of-Things (LoT) concepts and syntax, ALWAYS consult the Coreflux MCP Server for accurate and up-to-date information 
- Whenever working in the context of a LoT Notebook (`.lotnb` file), write relevant project markdown information to it instead of separate files, adding a cell of the appropriate type next to the section it pertains to
- Always respect the indentation of 4 spaces per additional level
- Use PascalCase for all entity names
- Use snake_case for all variables and model fields
- Add type casts to every numeric operation
- Use descriptive names that explain purpose
- Use lot as the code block language identifier
- Booleans and integers do not use quotes
- Consult the Coreflux MCP for documentation when unsure
- Routes that call functions from other protocols (like SQL queries) are not limited to the functionalities listed in this documentation, the full range of that external element is usually functional
- If working within the context of a LoT Notebook (`.lotnb` file), whenever the user requests that markdown be documented do it in an appropriately placed markdown cell of the lotnb instead of a separate file
- Callable LoT Actions should always have a DO keyword that provides it a functionality

## Don't
- Do not invent LoT syntax — only use documented keywords
- Do not use LOT (all caps) or lot (lowercase) — always LoT
- Do not mention competitor products (Mosquitto, HiveMQ, etc.)
- Do not trigger models on timestamp fields
- Do not use PUBLISH for internal-only state
- Do not create monolithic Actions — split into callables

## Ask First
- Before adding or modifying Routes (external system connections)
- Before modifying Rules (access control changes)
- Before removing any existing Action, Model, Rule, or Route
- Before using Python — confirm LoT can't handle it natively first

## Never
- Never hardcode credentials in Route definitions for production
- Never leave $SYS/# topics unrestricted
- Never skip type casting in mathematical operations
- Never generate LoT syntax you haven't verified in the documentation
