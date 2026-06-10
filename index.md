---
layout: home
author_profile: true
excerpt: "The EU-funded AI4SoilHealth (AI4SH) database holds soil data from field sampling and multiple analytical instruments. This site documents how to load that data using the Xspatula framework."
---

# Loading AI4SH data with Xspatula

The EU-funded AI4SoilHealth (AI4SH) project database stores soil data collected from field sampling across Europe, analysed by wet laboratory, near-infrared, mid-infrared, LIBS, and other spectrometers. To accommodate this data with [FAIR (Findability, Accessibility, Interoperability, and Reuse)][fair] principles the [Xspatula framework][setup_core_db_docs_framework] provides a comprehensive PostgreSQL database and a JSON-driven Python workflow for loading data into it.

## Prerequisites

Before loading data you must have completed two earlier stages:

1. **Database setup** — schemas and tables created; see [Setup core db][setup_core_db_docs]
2. **Process registration** — all framework processes registered; see [Setup processes][setup_process]

You also need to clone or download the AI4SH data loading package:

```bash
git clone https://github.com/xspatula/load_ai4sh_db
```

## Data loading overview

Data is loaded in a mandatory sequence — each stage depends on records from the previous one:

| Stage | Notebook | Pages |
|---|---|---|
| [Utility data][utility] | `load_ai4sh_utility_data.ipynb` | 7 |
| [Dataset metadata][dataset_meta] | `load_ai4sh_dataset_meta.ipynb` | 7 |
| [Sample data][sample] | `load_ai4sh_sample_data.ipynb` | 4 |
| [Wetlab data][wetlab] | `load_ai4sh_wetlab_data.ipynb` | 4 |
| [Spectral data][spectra] | `load_ai4sh_spectral_data.ipynb` | 13 |

Alternatively, all stages can be run from a [single notebook][all_data] (`load_ai4sh_data.ipynb`).

## Two-step pattern

Every data loading operation follows the same translate-then-manage pattern:

```
Excel source data
      ↓  [translate cell]
JSON process files
      ↓  [manage cell]
PostgreSQL database
```

The translate step reads Excel files and writes JSON process files to disk. The manage step reads those JSON files and executes them against the database. The two steps are decoupled so you can inspect the JSON before committing to the database.

## Acknowledgments and Funding

This work was done as part of the AI4SoilHealth project, funded by the European Union's Horizon Europe Research and Innovation Programme under Grant Agreement No. 101086179.

_Funded by the European Union. The views and opinions expressed are those of the authors only and do not necessarily reflect those of the European Union or the European Research Executive Agency._

[fair]: https://www.go-fair.org/fair-principles/
[setup_core_db_docs]: https://xspatula.github.io/setup_core_db_docs/
[setup_core_db_docs_framework]: https://xspatula.github.io/setup_core_db_docs/framework/
[setup_process]: /setup_process/
[utility]: /utility/
[dataset_meta]: /dataset_meta/
[sample]: /sample/
[wetlab]: /wetlab/
[spectra]: /spectra/
[all_data]: /all_data/
