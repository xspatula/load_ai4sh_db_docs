---
title: "Load Dataset Metadata"
layout: single
sidebar:
  nav: "dataset_meta"
excerpt: "Overview of the dataset metadata files and the required sequence for inserting them into the AI4SH database."
permalink: /dataset_meta/
author_profile: false
date: 2026-06-10 08:00:00 +0200
last_modified_at: 2026-06-10 08:00:00 +0200
---

Dataset metadata describes the provenance of soil observations: who collected them, under what campaign, and at which sampling locations. This metadata must be loaded before any sample or observation data.

All dataset metadata loading is driven by the notebook `load_ai4sh_dataset_meta.ipynb`.

## Required loading sequence

1. Translate dataset
2. Manage data source
3. Manage persons
4. Manage dataset
5. Manage campaign
6. Manage sampling log

_Content to be written._
