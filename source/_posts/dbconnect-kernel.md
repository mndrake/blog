---
title: dbconnect-kernel
date: 2020-02-22 21:33:35
tags: 
  - jupyter
  - dbconnect
  - databricks
---

This is an overview of how to create an Jupyter kernel that when selected will automatically create a connection to an already preconfigured cluster within Databricks.

## Initial Setup

Follow the instructions described on the Databricks [documentation site](https://docs.databricks.com/dev-tools/databricks-connect.html) to create a cluster within a databricks workspace and local conda environment. If you prefer to use a conda `environment.yml` file here is what I used to create a local conda environment named `dbconnect`. **Note this environment file assumes a databricks runtime of 6.2.**

<script src="https://gist.github.com/mndrake/d3ff5ac056b1c4dc164a3c70915ce39a.js"></script>

## Create kernel for conda environment

Activate the `dbconnect` conda environment and then create the jupyter kernal

```{bash}
conda activate dbconnect
ipython kernel install --name "dbconnect" --user
```

