---
title: "Load Spectral Data"
layout: single
sidebar:
  nav: "spectra"
excerpt: "Overview of spectral data in the AI4SH database. All data is converted to and stored as ascending reflectance. Covers FOSS DS2500, Neospectra, FTIR, and LIBS instruments."
permalink: /spectra/
author_profile: false
date: 2026-06-10 08:00:00 +0200
last_modified_at: 2026-06-10 08:00:00 +0200
---

The AI4SH database holds spectral data from four instruments. All spectra are converted to and stored as ascending wavelength reflectance regardless of the original instrument format.

All spectral data loading is driven by the notebook `load_ai4sh_spectral_data.ipynb`.

| Instrument | Wavelength range | Notes |
|---|---|---|
| FOSS DS2500 | NIR | Ascending in source and DB |
| Neospectra | NIR | Ascending in source and DB |
| Bruker Invenio (FTIR) | MIR | Descending in source, ascending in DB |
| FOSS Micral (LIBS) | UV–NIR | Descending in source, ascending in DB |

_Content to be written._
