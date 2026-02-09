# Data Dictionary
## Maji Ndogo Water Infrastructure Transparency Dashboard

This document describes the structure, relationships, and contents of all tables used in the Power BI data model.

---

## Table of Contents
1. [Data Model Overview](#data-model-overview)
2. [Fact Tables](#fact-tables)
3. [Dimension Tables](#dimension-tables)
4. [Relationships](#relationships)
5. [Data Types & Formats](#data-types--formats)

---

## Data Model Overview

### Schema Type
**Star Schema** - One central fact table connected to multiple dimension tables

### Tables
- **Fact Table**: `project_progress` (25,369 rows)
- **Dimension Tables**: 
  - `location` (27,628 rows)
  - `water_source` (27,628 rows)
  - `infrastructure_cost` (5 rows)
  - `vendors` (30 rows)
  - `well_pollution` (19,814 rows)

### Data Grain
Each row in `project_progress` represents one water source improvement project.

---

## Fact Tables

### project_progress

**Description**: Central fact table tracking each water source improvement project from planning through completion.

**Grain**: One row per water source improvement project

**Row Count**: 25,369

| Column Name | Data Type | Description | Example Value | Notes |
|------------|-----------|-------------|---------------|-------|
| `Project_id` | Integer | Unique project identifier | 12345 | Primary key |
| `source_id` | Text | Foreign key to water_source | AkRu20371224 | Links to water source |
| `town` | Text | Town or rural area name | Rural_Sokoto, Zuri | Includes province suffix |
| `Improvements_required` | Text | Type of improvement needed | Drill well, Install RO filter | Detailed description |
| `Aggregated_improvements` | Text | Grouped improvement category | Drill well, Install public tap(s) | For reporting/filtering |
| `source_status` | Text | Project status | Complete, Backlog | Two values only |
| `date_started` | Date | Project start date | 2023-01-15 | Format: YYYY-MM-DD |
| `date_of_completion` | Date | Project completion date | 2023-03-22 | Null if not complete |
| `cost` | Currency | Actual project cost (USD) | $11,575.00 | Includes all expenses |
| `assigned_vendor` | Text | Vendor ID | MBS605, ERI893 | Foreign key to vendors |
| `budgeted_improvement_cost` | Currency | Estimated cost (calculated) | $10,500.00 | Varies by location/type |

**Calculated Columns**:
- `budgeted_improvement_cost`: Estimated cost using rural adjustment if applicable
- `project_duration`: Days between start and completion dates

**Key Filters**:
- `source_status` for completed vs. pending
- `date_of_completion` for time-based analysis
- `town` for geographic filtering
- `Aggregated_improvements` for improvement type analysis

---

## Dimension Tables

### location

**Description**: Geographic information for all water sources and towns

**Grain**: One row per water source location

**Row Count**: 27,628

| Column Name | Data Type | Description | Example Value | Notes |
|------------|-----------|-------------|---------------|-------|
| `location_id` | Integer | Unique location identifier | 12345 | Primary key |
| `town_name` | Text | Original town name | Harare | Without province suffix |
| `province_name` | Text | Province name | Sokoto, Akatsi | One of 5 provinces |
| `location_type` | Text | Urban or Rural | Rural, Urban | For cost analysis |

**Relationships**: Links to `water_source` via `location_id`

**Note**: Town names in `project_progress` include province suffixes (e.g., "Harare-K" for Kilimani's Harare), while this table contains base names.

---

### water_source

**Description**: Details about each water source serving the population

**Grain**: One row per water source

**Row Count**: 27,628

| Column Name | Data Type | Description | Example Value | Notes |
|------------|-----------|-------------|---------------|-------|
| `source_id` | Text | Unique source identifier | AkRu20371224 | Primary key |
| `location_id` | Integer | Foreign key to location | 12345 | Links to location table |
| `type_of_water_source` | Text | Source category | well, river, shared_tap, tap_in_home | Five types total |
| `number_of_people_served` | Integer | Population using this source | 956 | For impact calculations |
| `Average_queue_time` | Integer | Avg wait time in minutes | 45 | Only for shared_tap type |

**Relationships**:
- Links to `location` via `location_id`
- Links to `project_progress` via `source_id`
- Links to `well_pollution` via `source_id` (for well types only)

**Water Source Types**:
1. `well` - Underground water wells (may need cleaning/filtering)
2. `river` - Surface water from rivers (needs well drilling)
3. `shared_tap` - Community taps (may need more taps or queue reduction)
4. `tap_in_home` - Individual household connections (ideal state)
5. `broken infrastructure` - Non-functioning infrastructure (needs repair)

---

### infrastructure_cost

**Description**: Unit costs for different types of improvements

**Grain**: One row per improvement type

**Row Count**: 5

| Column Name | Data Type | Description | Example Value | Notes |
|------------|-----------|-------------|---------------|-------|
| `Improvement` | Text | Improvement category | Drill well | Primary key |
| `unit_cost_USD` | Currency | Base cost in urban areas | $8,000.00 | USD |
| `Rural_adjusted_cost` | Currency | Increased cost for rural | $12,000.00 | ~1.5x urban cost |

**Improvement Types**:
1. `Drill well` - New well drilling
2. `Install public tap(s)` - Community tap installation
3. `Install RO filter` - Reverse osmosis filtration
4. `Install UV and RO filter` - Combined UV and RO treatment
5. `Repair infrastructure` - Fix broken systems

**Relationships**: Links to `project_progress` via `Aggregated_improvements` = `Improvement`

**Cost Adjustment**: Rural costs are higher due to:
- Difficult terrain access
- Longer travel distances
- Specialized equipment needs
- Limited local labor availability

---

### vendors

**Description**: Information about contractors performing improvements

**Grain**: One row per vendor company

**Row Count**: 30

| Column Name | Data Type | Description | Example Value | Notes |
|------------|-----------|-------------|---------------|-------|
| `assigned_vendor` | Text | Vendor ID | MBS605 | Primary key |
| `vendor_name` | Text | Company name | Mombasa Borehole Services | Full legal name |
| `specialization` | Text | Type of work performed | Groundwater Extraction | One of 4 types |
| `owner` | Text | Company owner name | Amara Okonkwo | For reference |

**Vendor Specializations**:
1. `Groundwater Extraction` - Drill wells
2. `Water Distribution System Installation` - Install taps
3. `Water Purification System Installation` - Install RO/UV filters
4. `Civil Infrastructure Assessment` - Repair broken infrastructure

**Relationships**: Links to `project_progress` via `assigned_vendor`

**Key Vendors** (by performance):
- **ERI893** (Entebbe RO Installers): Most efficient filter installer
- **MBS605** (Mombasa Borehole Services): Works in difficult Sokoto terrain
- **OW290** (Ouagadougou Waterworks): Less efficient routing

---

### well_pollution

**Description**: Water quality test results for wells

**Grain**: One row per well water source

**Row Count**: 19,814 (subset of water_source where type = "well")

| Column Name | Data Type | Description | Example Value | Notes |
|------------|-----------|-------------|---------------|-------|
| `source_id` | Text | Well identifier | AkRu20371224 | Foreign key |
| `results` | Text | Test result | Clean, Contaminated: Biological | Two categories |

**Test Results**:
1. `Clean` - Safe for drinking, no treatment needed
2. `Contaminated: Biological` - Needs filtration/treatment

**Relationships**: Links to `water_source` via `source_id`

**Usage**: Determines which wells need RO/UV filters vs. those that are already acceptable

---

## Relationships

### Relationship Diagram

```
location (1) ────────── (*) water_source
                              │
                              │ source_id
                              │
                              ├─────── (*) project_progress ──── (*) vendors
                              │              │
                              │              │ Aggregated_improvements
                              │              │
                              │              └──── (*) infrastructure_cost
                              │
                              └─────── (*) well_pollution
```

### Detailed Relationships

| From Table | From Column | To Table | To Column | Cardinality | Cross-filter |
|-----------|-------------|----------|-----------|-------------|--------------|
| water_source | source_id | project_progress | source_id | 1:* | Both |
| water_source | location_id | location | location_id | *:1 | Single |
| water_source | source_id | well_pollution | source_id | 1:1 | Both |
| infrastructure_cost | Improvement | project_progress | Aggregated_improvements | 1:* | Single |
| vendors | assigned_vendor | project_progress | assigned_vendor | 1:* | Single |

**Relationship Notes**:
- `project_progress` to `water_source`: Bi-directional for full interactivity
- `infrastructure_cost` uses text matching on improvement type
- Most relationships are 1:* (dimension to fact)

---

## Data Types & Formats

### Date Formats
**Standard**: YYYY-MM-DD (e.g., 2023-03-22)
**Power BI Type**: Date (not DateTime)

### Currency
**Currency**: USD ($)
**Format**: `$#,##0.00`
**Decimal Places**: 2

### Text
**Encoding**: UTF-8
**Case**: Mixed (proper nouns capitalized)

### Integers
**Format**: `#,##0`
**Negatives**: Not applicable in this dataset

### Percentages
**Storage**: Decimal (0.3359 = 33.59%)
**Display Format**: `0.00%`

---

## Data Quality Notes

### Missing Values
- `date_of_completion`: Null for incomplete projects (intentional)
- `cost`: Null for projects not yet started (intentional)
- `Average_queue_time`: Only populated for shared_tap type

### Data Validation
- All `source_id` values are unique
- All `date_started` ≤ `date_of_completion` (where both exist)
- All `cost` ≥ 0
- `source_status` only has two values: "Complete" or "Backlog"

### Data Refresh
- **Original Project**: Data updated quarterly (2023-2027)
- **Educational Dataset**: Static snapshot as of December 2027

---

## Cardinality Summary

| Table | Row Count | Storage Size | Key Type |
|-------|-----------|--------------|----------|
| project_progress | 25,369 | ~3.2 MB | Fact |
| water_source | 27,628 | ~2.8 MB | Dimension |
| location | 27,628 | ~1.1 MB | Dimension |
| well_pollution | 19,814 | ~0.8 MB | Dimension |
| vendors | 30 | ~0.01 MB | Dimension |
| infrastructure_cost | 5 | ~0.001 MB | Dimension |

**Total Model Size**: ~8 MB (approximately)

---

## Data Lineage

### Source System
**Original**: Excel workbook (`Md_water_services_data.xlsx`)
**Map Data**: JSON file (`MD_Full_map.json`)

### Transformation
**Tool**: Power Query Editor
**Key Transformations**:
1. Removed unused columns for optimization
2. Standardized date formats
3. Added province suffixes to town names
4. Created aggregated improvement categories

### Loading
**Method**: Import mode (not DirectQuery)
**Refresh Schedule**: Manual (educational project)

---

**Last Updated**: 2024  
**Data as of**: December 3, 2027  
**Dataset Version**: Final (100% project completion)
