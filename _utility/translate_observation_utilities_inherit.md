---
title: "Translate Observation Utilities with Inheritance"
layout: single
sidebar:
  nav: "utility"
excerpt: "Translating provision, provision_indicator, and provision_serial_nr Excel files to JSON process files. These tables use __ notation in parameter keys to resolve foreign keys by name rather than ID."
permalink: /utility/translate_observation_utilities_inherit/
author_profile: false
date: 2026-06-10 08:00:00 +0200
last_modified_at: 2026-06-10 08:00:00 +0200
---

The fifth utility step translates the three inheritance-dependent observation utility tables: `provision`, `provision_indicator`, and `provision_serial_nr`. These tables link multiple utility records together and use a special `__` notation in parameter keys for foreign key resolution.

## Prerequisite

[Manage observation utilities] must be complete. The `apparatus`, `provider`, `method_tier`, `indicator`, `analysis_method`, and `unit` tables must all be populated before these files can be managed.

## What "inheritance" means here

The manage process files produced in this step contain parameter keys with a double underscore (`__`) separator, for example:

```
"provider_id__provider_name": "metrohm"
```

This tells the framework to look up the `provider` record where `provider_name = "metrohm"` and substitute its `provider_id` at insert time. The referenced record must already exist in the database — hence the dependency on the earlier manage step.

This mechanism avoids hard-coding database IDs in the JSON files. You work with human-readable names in the Excel source, and the framework resolves them to primary keys at runtime.

## Notebook cell

In `load_ai4sh_utility_data.ipynb`, the **Translate observation utilities with inheritance** cell runs:

```python
job_file = 'import_data/utility/job_translate_observation_utility_inherit.json'

structured_process_D, scheme_params_D = Initiate_process(notebook_path, scheme_file, job_file)

if structured_process_D is not None:
    Run_process(structured_process_D, scheme_params_D)
```

## Job file

**Path**: `./ai4sh/import_data/utility/job_translate_observation_utility_inherit.json`

```json
{
  "process": {
    "job_folder": "import_data/utility/observation",
    "process_sub_folder": "process",
    "pilot_file": "translate_observation_utility_inherit.txt"
  }
}
```

## Pilot file

**Path**: `./ai4sh/import_data/utility/observation/translate_observation_utility_inherit.txt`

```
provision.json
provision_indicator.json
provision_serial_nr.json
```

| File | Database table | Requires |
|---|---|---|
| `provision.json` | `observation_utility.provision` | `apparatus`, `provider`, `method_tier` |
| `provision_indicator.json` | `observation_utility.provision_indicator` | `provision`, `indicator`, `analysis_method`, `unit` |
| `provision_serial_nr.json` | `observation_utility.provision_serial_nr` | `provision` |

## Translate process file structure

Example for `provision.json`:

**Path**: `./ai4sh/import_data/utility/observation/process/provision.json`

```json
{
  "process": [
    {
      "overwrite": true,
      "process": "translate_tabular_data",
      "parameters": {
        "process": "manage_provision",
        "tabular_data_path": "../../../utility/observation/excel/provision.xlsx",
        "dst_path": "../../../utility/observation/manage_process"
      }
    }
  ]
}
```

## Source files

| Excel file | Description |
|---|---|
| `provision.xlsx` | Combinations of apparatus + provider + method tier (e.g. a specific spectrometer operated by a specific lab at a specific professionality level) |
| `provision_indicator.xlsx` | Links each provision to the indicators it can deliver, with method and unit |
| `provision_serial_nr.xlsx` | Optional serial numbers for distinguishing individual instruments within the same provision |

Source directory: `./ai4sh/import_data/utility/observation/excel/`

## Output

Three manage process files are written to `./ai4sh/import_data/utility/observation/manage_process/`:

- `manage_provision.json`
- `manage_provision_indicator.json`
- `manage_provision_serial_nr.json`

## Next step

Proceed to [Manage observation utilities with inheritance] to insert these records into the database.

[Manage observation utilities]: /utility/manage_observation_utilities/
[Manage observation utilities with inheritance]: /utility/manage_observation_utilities_inherit/
