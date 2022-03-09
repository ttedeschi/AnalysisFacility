# A prototype for interactive analysis

<p align="center">
    <img src="/prototype.gif"></img>
</p>

This project is a prototype for a data analysis system, CMS compliant. The main
targets are:

1. Reducing analysis "**time to insight**" (training time for newcomers included) with an **interactive** and user-friendly **UI**
2. **Single and easily accessible hub** to reduce the complexity and maintenance of multiple and slightly overlapping solutions
3. **Increasing** the system delivered **throughput** (`evts/s`)

## Current design

The environment is composed using:

- [JupyterHub](https://jupyter.org/hub) and [JupyterLab](https://jupyter.org) to manage the user interaction part of the infrastructure
- [Dask](https://dask.org/) to introduce the scaling over a batch system
- [XRootD](https://xrootd.slac.stanford.edu/) as data access protocol toward _AAA_

At the moment, it supports scaling over [HTCondor](https://htcondor.org/) clusters using a custom
[dask-jobqueue](http://jobqueue.dask.org/en/latest/) module.
