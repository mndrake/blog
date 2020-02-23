---
title: dbconnect-kernel
date: 2020-02-22 21:33:35
tags: 
  - jupyter
  - dbconnect
  - databricks
---

This is an overview of how to create an Jupyter kernel that when selected will automatically create a connection to an already preconfigured cluster within Databricks. Also the referenced paths below are the default paths that Jupyter and IPython use on OSX, see Jupyter and IPython docs for Windows and Linux paths if needed.

## Initial Setup

Follow the instructions described on the Databricks [documentation site](https://docs.databricks.com/dev-tools/databricks-connect.html) to create a cluster within a databricks workspace and local conda environment. If you prefer to use a conda `environment.yml` file here is what I used to create a local conda environment named `dbconnect`. **Note this environment file assumes a databricks runtime of 6.2.**

<script src="https://gist.github.com/mndrake/d3ff5ac056b1c4dc164a3c70915ce39a.js"></script>

## Create kernel for conda environment

Activate the `dbconnect` conda environment and then create the jupyter kernel

```sh
conda activate dbconnect
ipython kernel install --user --name dbconnect
```

You should now have a new jupyter kernel that references the python kernel in your `dbconnect` conda environment. 

Navigate to the your new kernel folder on a Mac this should be `~/Library/Jupyter/kernels/dbconnect` and edit `kernel.json`. Change `ipykernel_launcher` to `ipykernel` and add the reference to the profile dbconnect. You should end up with something similar to the file below.

<script src="https://gist.github.com/mndrake/96e550ae3005c33ab2563898d5b1fbcc.js"></script>


## Create profile for startup scripts

We are referecing a ipython profile that does not exist yet, so we need to create it before we are able to use our new kernel. Create the following directory `~/.ipython/profile_dbconnect/startup` and add the following file as `00-setup.py`. The contents of this script with execute everytime that the `dbconnect` kernel is selected with Jupyter. This allows us to automatically load any libraries, objects or custom functions we want everytime we use a kernel.

In this case we will initialize the following, mimicking the experience within a databricks notebook:
* `spark` our spark session
* `sc` our spark context
* `dbutils` is loaded by default
* `%sql` cell magic similar to databricks notebook

<script src="https://gist.github.com/mndrake/38315a83717fa6608dc17af700e9bd37.js"></script>

## Validate

You should now be able to launch `jupyter notebook` and now be able to select the `dbconnect` kernel and start using dbconnect without any additional configuration. This does require that your cluster is already running within databricks first though. In the future I may look into automating the creation and starting of a cluster from the client side, but hopefully this is a start to easing the use of `dbconnect` within Jupyter.
