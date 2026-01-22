# F1 Lakehouse Project

This project is a learning-focused data engineering project built around Formula 1 data.
The goal is to design and implement a small lakehouse-style pipeline and a results exploration app,
using tools and patterns commonly found in data platform teams.

The project is intentionally scoped to be runnable on a single VPS while still reflecting
real-world architecture decisions.

---

## Goals

- Learn and practice PySpark and Spark batch processing
- Understand lakehouse architecture (Bronze / Silver / Gold)
- Work with real external APIs and semi-structured data
- Build a backend + frontend application on top of analytics tables
- Prepare for a data platform / Databricks-style internship

---

## Data Sources

- Jolpica (Ergast-compatible) API  
  Used for seasons, races, drivers, constructors, results, and standings

- OpenF1 API  
  Used for sessions and lap time data

Data is ingested historically and refreshed daily using batch jobs.

---

## Architecture

The system is composed of four main parts:

1. External APIs providing raw Formula 1 data
2. PySpark batch jobs handling ingestion, transformation, and aggregation
3. Lakehouse storage using object storage and Delta tables
4. A results explorer application (FastAPI backend + React frontend)

![Architecture Diagram](diagrams/architecture.png)

---

## Data Pipeline

The data pipeline follows a classic Bronze / Silver / Gold pattern:

- Bronze: raw API payloads, stored as-is
- Silver: cleaned, normalized, typed datasets
- Gold: aggregated, analytics-ready tables used by the application

Jobs are executed daily and are independent from the application runtime.

![Pipeline Sequence Diagram](diagrams/sequence.png)

---

## Data Model

The core domain model focuses on races, drivers, results, and lap times.
Gold tables summarize race-level driver performance to support fast queries.

![Domain Model](diagrams/domain_model.png)

---

## Application

The application is split into two components:

- Backend: Python (FastAPI)
  - Exposes endpoints to query Gold tables
  - Handles filtering by season, race, and drivers

- Frontend: JavaScript (React)
  - Allows users to select seasons, races, and drivers
  - Displays tables and charts based on API responses

The application never triggers Spark jobs directly and only reads from Gold tables.

---

## Repository Structure

```
f1-lakehouse/
├── jobs/        # PySpark ingestion and transformation jobs
├── backend/     # FastAPI backend
├── frontend/    # React frontend
├── diagrams/    # Architecture diagrams
├── README.md
└── .gitignore
```

Large datasets and generated tables are intentionally excluded from version control.

---

## Notes

This project is not intended to be production-ready.
The focus is on understanding architecture, data flow, and design tradeoffs rather than
on completeness or performance optimization.
