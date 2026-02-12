# AGENTS.md

## Project Overview
This is a Coreflux LoT (Language of Things) project for [describe your system].

## Naming Conventions
- **Entities (Actions, Models, Rules, Routes):** PascalCase
- **Variables and model fields:** snake_case in quotes
- **Topics:** lowercase/slash-separated

## Topic Hierarchy
- sensors/    — Raw incoming data
- processed/  — Transformed data
- alerts/     — Threshold violations
- state/      — Internal persistent state

## Do
- Always type-cast in calculations (AS DOUBLE, AS INT)
- Use KEEP TOPIC for internal state, PUBLISH TOPIC for broadcast
- Use lot as the code block language identifier
- Consult the Coreflux MCP for documentation when unsure

## Don't
- Do not invent LoT syntax — only use documented keywords
- Do not use LOT (all caps) or lot (lowercase) — always LoT
- Do not create monolithic Actions — split into callable Actions
- Do not trigger models on timestamp fields
