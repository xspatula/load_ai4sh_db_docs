---
title: "Load Utility Data"
layout: single
sidebar:
  nav: "utility"
excerpt: "Overview of the utility data types and the required loading sequence before any observation data can be entered into the AI4SH database."
permalink: /utility/
author_profile: false
date: 2026-06-10 08:00:00 +0200
last_modified_at: 2026-06-10 08:00:00 +0200
---

Utility data provides the controlled vocabularies and reference catalogues that all observation data depends on. Every observation record must reference at least one utility entry — an apparatus, an indicator, a unit, a provision. Utility data must therefore be fully loaded before any dataset, sample, or observation data can be entered.

All utility loading is driven by a single notebook:

```
./ai4sh/import_data/load_ai4sh_utility_data.ipynb
```

## Two groups of utility data

Utility data is split into two groups with different purposes and storage locations.

### General utilities

General utilities contain reference data used across the whole framework, not just observations. For the AI4SH project this group contains a single table:

| Excel file | Database table | Description |
|---|---|---|
| `territory.xlsx` | `utility.territory` | Countries and regions (ISO codes) |

Source: `./ai4sh/import_data/utility/general/excel/`

### Observation utilities

Observation utilities are the catalogues that make soil observations FAIR-compliant — every instrument, method, unit, and indicator used in an observation must first be registered here.

There are two sub-groups within observation utilities:

**Independent** (no foreign key dependencies on other observation utilities):

| Excel file | Database table | Description |
|---|---|---|
| `analysis_method.xlsx` | `observation_utility.analysis_method` | Laboratory and field analysis methods |
| `apparatus.xlsx` | `observation_utility.apparatus` | Instruments and tools delivering data |
| `classification_order.xlsx` | `observation_utility.classification_order` | Highest Linnean taxonomy level |
| `license.xlsx` | `observation_utility.license` | Dataset and campaign licenses |
| `location_method.xlsx` | `observation_utility.location_method` | Geolocation methods |
| `method_tier.xlsx` | `observation_utility.method_tier` | Professionality level of a method |
| `preparation.xlsx` | `observation_utility.preparation` | Pre-analysis sample preparation |
| `preservation.xlsx` | `observation_utility.preservation` | Sample preservation methods |
| `provider.xlsx` | `observation_utility.provider` | Labs, services and instruments providing results |
| `quantity.xlsx` | `observation_utility.quantity` | Unambiguous quantity definitions |
| `setting_system.xlsx` | `observation_utility.setting_system` | Thematic frame for juxtapositions |
| `spatial_reference.xlsx` | `observation_utility.spatial_reference` | EPSG-based coordinate reference systems |
| `storage.xlsx` | `observation_utility.storage` | Sample storage methods |
| `transportation.xlsx` | `observation_utility.transportation` | Sample transport conditions |
| `unit.xlsx` | `observation_utility.unit` | Units of observation values |

**Dependent** (have foreign key requirements from the independent tables above):

| Excel file | Database table | Requires |
|---|---|---|
| `classification_family.xlsx` | `observation_utility.classification_family` | `classification_order` |
| `classification_genus.xlsx` | `observation_utility.classification_genus` | `classification_family` |
| `indicator.xlsx` | `observation_utility.indicator` | `quantity` |
| `juxtaposition.xlsx` | `observation_utility.juxtaposition` | `setting_system` |
| `profiling.xlsx` | `observation_utility.profiling` | `unit` |

**With inheritance** (depend on multiple observation utility tables and use `__` notation for FK lookup):

| Excel file | Database table | Requires |
|---|---|---|
| `provision.xlsx` | `observation_utility.provision` | `apparatus`, `provider`, `method_tier` |
| `provision_indicator.xlsx` | `observation_utility.provision_indicator` | `provision`, `indicator`, `analysis_method`, `unit` |
| `provision_serial_nr.xlsx` | `observation_utility.provision_serial_nr` | `provision` |

Source: `./ai4sh/import_data/utility/observation/excel/`

## Required loading sequence

The six notebook cells must be run in this order:

1. **[Translate general utilities]** — Excel → JSON for `territory`
2. **[Manage general utilities]** — Insert `territory` into the database
3. **[Translate observation utilities]** — Excel → JSON for independent and dependent tables
4. **[Manage observation utilities]** — Insert those tables into the database
5. **[Translate observation utilities with inheritance]** — Excel → JSON for `provision`, `provision_indicator`, `provision_serial_nr`
6. **[Manage observation utilities with inheritance]** — Insert those tables into the database

[Translate general utilities]: /utility/translate_general_utilities/
[Manage general utilities]: /utility/manage_general_utilities/
[Translate observation utilities]: /utility/translate_observation_utilities/
[Manage observation utilities]: /utility/manage_observation_utilities/
[Translate observation utilities with inheritance]: /utility/translate_observation_utilities_inherit/
[Manage observation utilities with inheritance]: /utility/manage_observation_utilities_inherit/
