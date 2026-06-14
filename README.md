# COA Connectivity Lab — `data-management`

Part of the **[Eco Connectivity Workflow](https://github.com/coa-connectivity-lab/eco-connectivity-workflow)** — a modular, reproducible stack for modelling ecological connectivity, integrating database management, machine learning, GIS visualisation, and biodiversity data ingestion.

---

## The Modular Stack

The initiative is structured as a **modular GitHub organisation**, allowing multiple teams to work in parallel while keeping the full workflow reproducible and traceable.

| Module | Purpose | Repository |
|--------|---------|------------|
| **Database** | PostgreSQL/PostGIS schema and workflow views | [`db-schema`](https://github.com/coa-connectivity-lab/db-schema) |
| **Machine Learning** | Python scripts for resistance modelling and connectivity analysis | [`ml-models`](https://github.com/coa-connectivity-lab/ml-models) |
| **GIS Projects** | QGIS projects, styles, and figure templates | [`qgis-projects`](https://github.com/coa-connectivity-lab/qgis-projects) |
| **Data Management** | Ingests GBIF occurrence records and environmental rasters | [`data-management`](https://github.com/coa-connectivity-lab/data-management) ← **you are here** |
| **Reproducibility Logs** | Tracks scenarios, git commits, and outputs for traceability | [`reproducibility-logs`](https://github.com/coa-connectivity-lab/reproducibility-logs) |

```mermaid
flowchart TD
    A[Data Team] -->|Ingests detections & rasters| B[Database]
    B -->|Stores schema & views| C[ML Team]
    C -->|Predicts resistance & connectivity| B
    C --> D[GIS Team]
    D -->|Visualises & exports maps| E[Figures & Publications]
    A --> F[Reproducibility Logs]
    B --> F
    C --> F
```

### How the teams fit together

1. **Data Team** — prepares and ingests GBIF occurrence records, environmental rasters, and camera trap data. This is the repository that team works in.
2. **Database (DB) Team** — maintains the PostGIS schema, tables, and workflow views in [`db-schema`](https://github.com/coa-connectivity-lab/db-schema).
3. **ML Team** — runs resistance and connectivity models in [`ml-models`](https://github.com/coa-connectivity-lab/ml-models), storing outputs back in the database.
4. **GIS Team** — connects to the database in QGIS via [`qgis-projects`](https://github.com/coa-connectivity-lab/qgis-projects), visualises results, and exports maps.
5. **Reproducibility Logs** — tracks all actions, scenario IDs, and git commits for full traceability.

---

## This Repository — `data-management`

This repo covers everything from raw GBIF downloads to analysis-ready species layers, organised into nine ecological functional groups. Outputs feed directly into the ML and GIS modules.

### Pipeline notebooks (run in order)

| Notebook | Role |
|----------|------|
| `01_data_preparation.ipynb` | Natura 2000 clip + study area buffer |
| `02_gbif_processing.ipynb` | Per-species occurrence download + cleaning |
| `03_collate_all_species.ipynb` | Collates all species into a master GeoDataFrame, splits by functional group |
| `04_resistance_surface.ipynb` | Land cover → per-species resistance raster |
| `05_omniscape.ipynb` | Omniscape.jl run via juliacall/subprocess |
| `06_analysis.ipynb` | Corridors, zonal stats, co-benefit rasters |

---

## Contributing and Joining the Project

We welcome researchers, conservation practitioners, GIS specialists, data scientists, software developers, and students interested in ecological connectivity, biodiversity conservation, and open science.

### How to Contribute

You can contribute by:

* Reporting bugs or workflow issues
* Improving documentation
* Adding new species datasets
* Developing resistance surface methodologies
* Improving connectivity analyses
* Contributing QGIS styles and cartographic outputs
* Extending reproducibility and automation tools

Please open an Issue or Pull Request on [`data-management`](https://github.com/coa-connectivity-lab/data-management) to discuss proposed changes, or on [`eco-connectivity-workflow`](https://github.com/coa-connectivity-lab/eco-connectivity-workflow) for cross-module discussions.

### Join the COA Connectivity Lab

The COA Connectivity Lab is an open research initiative focused on reproducible ecological connectivity modelling and conservation planning.

If you would like to:

* Collaborate on research projects
* Contribute code or analyses
* Develop new ecological workflows
* Participate in student projects or internships
* Join the organisation as a collaborator

please contact the project owner:

**Linda Angulo Lopez**

GitHub: [@lindangulopez](https://github.com/lindangulopez)

Email: [angulo.linda@gmail.com](mailto:angulo.linda@gmail.com)

Researchers and contributors from academia, government agencies, NGOs, and the open-source community are encouraged to get involved.

### Code of Conduct

We are committed to maintaining a welcoming, inclusive, and collaborative environment. All contributors are expected to engage respectfully and constructively.

---

## Quick Start

### Technical Stack

* Python ≥ 3.9
* GNU Guix (primary environment) or conda
* QGIS ≥ 3.28 (for project visualisation)
* PostgreSQL + PostGIS (or Docker setup — see [`db-schema`](https://github.com/coa-connectivity-lab/db-schema))
* Git

### 1. Clone the Repository

```bash
git clone https://github.com/coa-connectivity-lab/data-management
cd data-management
```

### 2. Create the Reproducible Environment

Using GNU Guix:

```bash
guix shell -m env/manifest.scm
```

or:

```bash
./setup_env.sh
```

Verify installation:

```bash
python --version
jupyter --version
```

### 3. Add Source Data

#### GBIF Occurrence Records

Download Darwin Core Archives (DwC-A) from GBIF for the species of interest and place them in:

```text
data/raw/gbif/
```

Example:

```text
data/raw/gbif/
├── Canis_lupus.zip
├── Lynx_pardinus.zip
├── Cervus_elaphus.zip
└── ...
```

#### Natura 2000 Data

Download the latest Natura 2000 dataset from the European Environment Agency and place it in:

```text
data/raw/eea/natura2000/
```

Expected files:

```text
Natura2000_end2024.shp
Natura2000_end2024.gpkg
metadata.xml
```

### 4. Configure Species

Edit `src/config/ecosystem.py` to define:

* species list
* ecological groups
* resistance parameters
* dispersal assumptions

### 5. Run the Workflow

Execute the complete pipeline:

```bash
python run_all_species.py
```

The pipeline automatically:

1. Extracts GBIF occurrence records.
2. Cleans and filters coordinates.
3. Creates per-species occurrence layers.
4. **Collates all species into a master GeoDataFrame and splits by functional group.**
5. Generates resistance surfaces.
6. Runs Omniscape connectivity analyses.
7. Produces multi-species connectivity products.
8. Exports maps, figures, and summary tables.

### 6. Inspect Outputs

Key outputs are written to `data/processed/`.

Species collation products (from `notebooks/03_collate_all_species.ipynb`):

```text
data/processed/
├── all_species_master.parquet          # full master GeoDataFrame (EPSG:4326)
└── groups/
    ├── terrestrial_carnivores.gpkg
    ├── large_herbivores.gpkg
    ├── birds.gpkg
    ├── aquatic_and_freshwater.gpkg
    ├── pollinators.gpkg
    ├── lepidoptera.gpkg
    ├── vegetation.gpkg
    ├── decomposers.gpkg
    ├── invasive_species.gpkg
    └── translocation_species.gpkg      # all translocation-flagged species combined
```

Connectivity products:

```text
data/processed/connectivity/
├── {species}_current_flow.tif
├── {species}_normalized_current.tif
├── multispecies_mean_connectivity.tif
└── multispecies_max_connectivity.tif
```

Maps and figures:

```text
outputs/maps/
outputs/figures/
outputs/tables/
```

### 7. Open the QGIS Project

```bash
qgis qgis/coa_connectivity.qgz
```

The project includes species occurrence layers, resistance surfaces, connectivity maps, and publication-ready cartographic layouts. For cross-module QGIS workflows, see [`qgis-projects`](https://github.com/coa-connectivity-lab/qgis-projects).

### Expected Runtime

| Step | Time |
|------|------|
| Data preparation | 5–15 min |
| GBIF processing | 5–30 min |
| Species collation | < 5 min |
| Resistance modelling | 10–60 min |
| Omniscape analysis | 30 min–several hours |
| Post-processing | 5–20 min |

Runtime depends on raster resolution, study area extent, and number of species analysed.

---

## Species Collation Notebook

`notebooks/03_collate_all_species.ipynb` reads all per-species outputs produced by the cleaning pipeline and consolidates them into a single analysis-ready GeoDataFrame. Run it after all per-species notebooks have completed.

### What it does

1. Iterates over the full species registry (54 species across 9 functional groups).
2. For each species, loads data in preference order: `.parquet` → `.gpkg` → `.csv`.
3. Attaches four metadata columns to every record:

| Column | Values |
|--------|--------|
| `species_name` | e.g. `Lynx pardinus` |
| `functional_group` | e.g. `Terrestrial Carnivores` |
| `medium` | `LAND`, `AIR`, or `WATER` |
| `translocation` | `True` / `False` |

4. Normalises all layers to EPSG:4326 and concatenates into `all_species_master.parquet`.
5. Splits the master into one GeoPackage per functional group plus a `translocation_species.gpkg` cross-group subset.
6. Exposes named GeoDataFrame variables (`gdf_carnivores`, `gdf_aquatic`, etc.) for direct use in downstream notebooks.

### Excluded species

The following species are skipped automatically — no manual edits required:

| Species | Reason |
|---------|--------|
| *Iberochondrostoma almacai* | No GBIF records |
| *Quercus suber* | No GBIF Portugal dataset |
| *Sylvia melanocephala* | Empty after coordinate uncertainty filter (> 5 000 m) |
| *Squalius torgalensis* | Empty after coordinate uncertainty filter |
| *Salmo salar* | Empty after coordinate uncertainty filter |

### Adding a new species

1. Download the DwC-A archive from GBIF and place it in `data/raw/gbif/`.
2. Run (or adapt) `02_gbif_processing.ipynb` for the new species — this produces the cleaned `.parquet` / `.gpkg` / `.csv` under `data/processed/species/<Species_name>/`.
3. Add an entry to the `SPECIES_REGISTRY` list in `03_collate_all_species.ipynb`:

```python
dict(
    species='Genus species',
    folder='Genus_species',        # matches the subdirectory name
    functional_group='Birds',      # one of the nine defined groups
    medium='AIR',                  # LAND | AIR | WATER
    translocation=False,           # True if part of the translocation framework
),
```

4. Re-run `03_collate_all_species.ipynb` to regenerate the master file and all group GeoPackages.

---

## Publishing Cleaned Occurrence Data to GBIF

If you have collected new occurrence records (field surveys, camera traps, acoustic monitoring, etc.) that are not yet in GBIF, you can publish them back through the GBIF network. This makes your data citable, assigns a DOI, and allows others to build on your work.

GBIF data is published through **Endorsed Data Publishers** — typically universities, museums, research institutes, or NGOs that hold a formal agreement with GBIF. Individual researchers publish through an affiliated institution.

### Step-by-step

#### 1. Find or register a publishing institution

Go to [https://www.gbif.org/become-a-publisher](https://www.gbif.org/become-a-publisher) and check whether your institution is already a registered publisher. If it is, contact your institution's data manager to request that your dataset be published under their account. If not, submit a registration request — GBIF typically reviews these within a few days to a few weeks.

#### 2. Format your data as Darwin Core

GBIF accepts data in **Darwin Core Archive (DwC-A)** format — the same format used by the downloads this project consumes. Minimal required fields:

| Darwin Core field | Notes |
|-------------------|-------|
| `occurrenceID` | Unique identifier per record |
| `scientificName` | Full species name |
| `decimalLatitude` | WGS84 |
| `decimalLongitude` | WGS84 |
| `eventDate` | ISO 8601 (e.g. `2024-05-12`) |
| `basisOfRecord` | e.g. `HumanObservation`, `MachineObservation` |
| `countryCode` | ISO 3166-1 alpha-2 (e.g. `PT`) |

Strongly recommended: `coordinateUncertaintyInMeters`, `geodeticDatum`, `taxonKey` (GBIF backbone), `recordedBy`, `institutionCode`, `datasetName`.

#### 3. Use the GBIF Integrated Publishing Toolkit (IPT)

The **IPT** is a free, open-source web application that converts your CSV or database into a DwC-A, registers it with GBIF, and issues a DOI.

- Your institution may already host an IPT instance — ask your data manager.
- GBIF also provides a hosted IPT for publishers who cannot run their own: request access via [ipt@gbif.org](mailto:ipt@gbif.org).
- Self-hosting documentation: [https://ipt.gbif.org/manual/](https://ipt.gbif.org/manual/)

Workflow inside the IPT:

1. Create a new **Resource** and select *Occurrence* as the type.
2. Upload your CSV and map columns to Darwin Core terms.
3. Set the dataset metadata (title, description, sampling methods, licence — **CC BY 4.0** is recommended).
4. Click **Publish** — the IPT packages the DwC-A, registers it with GBIF, and issues a DOI.

#### 4. Add the dataset DOI to this project

Record the dataset DOI in `docs/01_data_sources.md` alongside the existing GBIF download DOIs so the full provenance chain is documented.

#### 5. Licence recommendation

Use **CC BY 4.0** or **CC0** wherever possible. GBIF requires one of: CC0, CC BY, or CC BY-NC. Restrictive licences reduce discoverability and reuse.

### Useful links

| Resource | URL |
|----------|-----|
| Become a publisher | https://www.gbif.org/become-a-publisher |
| Darwin Core terms reference | https://dwc.tdwg.org/terms/ |
| IPT manual | https://ipt.gbif.org/manual/ |
| GBIF data quality flags | https://data-blog.gbif.org/post/gbif-flags/ |
| Biodiversity Data Journal | https://bdj.pensoft.net/ |

> **Tip — data papers:** for significant new datasets, consider submitting a *data paper* to [Biodiversity Data Journal](https://bdj.pensoft.net/) or [ZooKeys](https://zookeys.pensoft.net/). The IPT can auto-generate a draft manuscript from your dataset metadata. Data papers are peer-reviewed, indexed, and give your dataset an additional citable output beyond the GBIF DOI.

---

## Upcoming Enhancements

* **Docker & Docker Compose** — launch a full environment (PostGIS + Python + TimescaleDB) with one command.
* **CI/CD** — GitHub Actions to test SQL views, Python scripts, and QGIS project validity.
* **Cloud Integration** — large raster datasets stored as Cloud-Optimized GeoTIFFs (COGs) on S3 or GCS, referenced in the database.

---

## Reproducibility Checklist

Before publishing results, confirm:

* [ ] Raw GBIF archives remain unchanged.
* [ ] Natura 2000 source files are preserved.
* [ ] Processing was executed inside the Guix environment.
* [ ] All outputs use EPSG:3035.
* [ ] Workflow commit hash is recorded.
* [ ] Species configuration file is archived.
* [ ] Generated maps and statistics correspond to the recorded workflow version.

---

## License

GNU GPL v3 — freely explore and adapt this workflow.
