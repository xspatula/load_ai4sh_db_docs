---
title: "Translate Observation Utilities"
layout: single
sidebar:
  nav: "utility"
excerpt: "Translating observation utility Excel catalogues to JSON process files. Covers 15 independent tables and 5 FK-dependent tables, all in a single notebook cell."
permalink: /utility/translate_observation_utilities/
author_profile: false
date: 2026-06-10 08:00:00 +0200
last_modified_at: 2026-06-10 08:00:00 +0200
---

The third utility step translates all observation utility catalogues — except the inheritance-dependent ones — from Excel to JSON process files. Twenty tables are handled in a single cell: 15 with no foreign key dependencies, followed by 5 that depend on tables from the same group.

## Prerequisite

[Manage general utilities] must be complete before this step.

## Notebook cell

In `load_ai4sh_utility_data.ipynb`, the **Translate observation utilities** cell runs:

```python
job_file = 'import_data/utility/job_translate_observation_utility.json'

structured_process_D, scheme_params_D = Initiate_process(notebook_path, scheme_file, job_file)

if structured_process_D is not None:
    Run_process(structured_process_D, scheme_params_D)
```

## Job file

**Path**: `./ai4sh/import_data/utility/job_translate_observation_utility.json`

```json
{
  "process": {
    "job_folder": "import_data/utility/observation",
    "process_sub_folder": "process",
    "pilot_file": "translate_observation_utility.txt"
  }
}
```

## Pilot file

**Path**: `./ai4sh/import_data/utility/observation/translate_observation_utility.txt`

The pilot file lists the translate process files in the required order — independent tables first, then the five FK-dependent tables. Lines beginning with `#` are comments and are skipped.

**Independent tables** (no foreign key dependencies):

```
analysis_method.json
apparatus.json
classification_order.json
license.json
location_method.json
method_tier.json
preparation.json
preservation.json
provider.json
quantity.json
setting_system.json
spatial_reference.json
storage.json
transportation.json
unit.json
```

**FK-dependent tables** (must follow the independent tables listed above):

```
classification_family.json
classification_genus.json
indicator.json
juxtaposition.json
profiling.json
```

| File | Requires |
|---|---|
| `classification_family.json` | `classification_order` |
| `classification_genus.json` | `classification_family` |
| `indicator.json` | `quantity` |
| `juxtaposition.json` | `setting_system` |
| `profiling.json` | `unit` |

## Translate process file structure

Each entry in the pilot file is a translate process file in `./ai4sh/import_data/utility/observation/process/`. All files share the same structure. Example for `apparatus.json`:

**Path**: `./ai4sh/import_data/utility/observation/process/apparatus.json`

```json
{
  "process": [
    {
      "process": "translate_tabular_data",
      "overwrite": true,
      "parameters": {
        "process": "manage_apparatus",
        "tabular_data_path": "../../../utility/observation/excel/apparatus.xlsx",
        "dst_path": "../../../utility/observation/manage_process"
      }
    }
  ]
}
```

Each translate process file reads its Excel source and writes a manage process file to:

```
./ai4sh/import_data/utility/observation/manage_process/
```

## Source files

All Excel source files are in `./ai4sh/import_data/utility/observation/excel/`. One `.xlsx` file per table. Column headers must match the target database table column names exactly.

## Output

Running this cell generates 20 manage process files in `./ai4sh/import_data/utility/observation/manage_process/`, one per table. The cell prints the paths to all files created. These are then executed in the next step.

## Next step

Proceed to [Manage observation utilities] to insert these catalogues into the database.

[Manage general utilities]: /utility/manage_general_utilities/
[Manage observation utilities]: /utility/manage_observation_utilities/
