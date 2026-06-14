# COA Connectivity Lab — `data-management`

A modular, reproducible workflow for modelling ecological connectivity, integrating database management, machine learning, GIS visualisation, and biodiversity data ingestion.

---

## Overview

The **Eco Connectivity Workflow** is a modular GitHub-based research stack designed to support scalable, reproducible ecological connectivity modelling.

It enables multiple teams to work in parallel while maintaining full provenance tracking, reproducibility, and interoperability across all stages of the pipeline.

The system transforms raw biodiversity and environmental data into **connectivity maps, resistance surfaces, and spatial conservation products**.

---

## The Modular Stack

| Module | Purpose | Repository |
|--------|---------|------------|
| **Database** | PostgreSQL/PostGIS schema and workflow views | https://github.com/coa-connectivity-lab/db-schema |
| **Machine Learning** | Resistance modelling and connectivity analysis | https://github.com/coa-connectivity-lab/ml-models |
| **GIS Projects** | QGIS projects, styles, and cartographic outputs | https://github.com/coa-connectivity-lab/qgis-projects |
| **Data Management** | GBIF ingestion, raster preprocessing, and species harmonisation | https://github.com/coa-connectivity-lab/data-management |
| **Reproducibility Logs** | Scenario tracking, outputs, and execution provenance | https://github.com/coa-connectivity-lab/reproducibility-logs |

---

## System Architecture

```mermaid
flowchart TD
    A[Data Team] -->|Ingest GBIF, rasters, field data| B[Database]
    B -->|PostGIS schema + spatial views| C[ML Models]
    C -->|Resistance & connectivity outputs| B
    C --> D[GIS/QGIS]
    D -->|Maps & spatial products| E[Figures & Publications]
    A --> F[Reproducibility Logs]
    B --> F
    C --> F
    D --> F