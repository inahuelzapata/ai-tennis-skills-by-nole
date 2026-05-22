ai-tennis-skills-by-nole

A collection of reusable AI skills for tennis workflows.

This repository is designed to store portable SKILL.md files that can be shared, versioned, improved, and reused across AI assistants or automation workflows.

Goal

Build a clean library of tennis-related AI skills focused on:

* AAT fixtures and results
* Club match-day communication
* Interclubes planning
* Fixture curation
* Instagram / social media content preparation
* Team formation scenarios
* Match reports and summaries

Repository Structure

ai-tennis-skills-by-nole/
├── README.md
└── skills/
    └── fixture-aat/
        └── SKILL.md

Skills

fixture-aat

Extracts and curates fixture information from an AAT entity URL.

Expected inputs:

* AAT entity URL
* Categories of interest
* Target date or weekend
* Optional local club name

Expected output:

* Curated list of upcoming series
* Category
* Date
* Time
* Opponent
* Local / visitor condition
* Notes about ignored or excluded categories

Skill Format

Each skill should live inside:

skills/<skill-name>/SKILL.md

A skill should include:

* Purpose
* Inputs
* Rules
* Output format
* Edge cases
* Examples

Naming Convention

Use lowercase kebab-case:

fixture-aat
tennis-story-fixture
team-formation-whatsapp
match-report

Philosophy

These skills should be practical, reusable, and opinionated.

The goal is not to make generic prompts, but to encode repeatable tennis workflows with clear rules so the AI produces consistent results every time.