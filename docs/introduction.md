<h1 style="margin-bottom: 0px">Building a testbed<br>for an Analysis Facility at INFN</h1>
<h5>a Prototype</h5>

## What

This is a R&D project aiming at building a testbed for an analysis facility at INFN. The testbed is intended for end-users (expert in data analysis) and developers in computing matters. As such it must be seen as a platform available for the exploitation of new analysis framework, running/porting existing analysis as well as exploiting new ideas for resources integration and provisioning. Some concrete examples are: 

1. __Testing/Benchmarking and comparing distinct approaches such as interactive vs batch like analysis__. From a user perspective this can be seen as the place where to test how a Jupyter big as a whole Tier2 can perform. 
2. __Validate and gain experience with new frameworks (RDF, Coffea etc)__. From a user perspective, this can be seen as the place where easily test available examples of analysis and how they look like when written in a column-wise fashion.
3. __Study new solution for transparent resource integration.__ From a computing developer this is the place where we provide a concrete implementation for "continuum model" where Grid, Cloud and HPC coexist transparently for the user. Also, here you can see how to virtually bring your resources (i.e. local cluster/nodes) and reuse all the rest of the stack with zero effort.
4. __Experimenting and developing new solutions for resource provisioning__ to overcome the grid "limitations". From a computing expert point of view, here is where suggest what to experiment in order to overcome some known current limitation

To this end, the prototype provides a user-friendly computational environment that pretends to simplify and facilitate the overall analysis process, enabling rapid processing of data eventually in a column-wise fashion. The technical services are based on open source industry standards. More in detail, they are based on Dask and Jupyter notebooks and rely on HTCondor for a transparent integration of dispersed (and heterogeneous) computing resources. It also guarantees resouce allocation based on fairshare (implemented thanks to HTCondor). The technical architecture is described [here](how_it_works/components.md).

Hardware wise, we currently we plan to be able to do all the initial activities using a very limited fraction of the existing funded resources (mostly at Tier2). In addition, we plan to rely on a limited amount of computing power provided by INFN-Cloud (actually cloud@CNAF) that on the one hand represents the seed (i.e. where to develop / test and prototype the analysis) for the AF and on the other hand is where we are deploying at the central services. The details about the currently available resources can be found [here](how_it_works/available_resources.md). All these explicit how the first objective of the project is not about purchasing any new hardware but rather about gain experience on new approaches to data analysis as well as to evolve the resource access strategies. Primarily, we would like to learn how to better exploit any available provider.
On the other side, it should appear rather obvious that we might understand that changes in the type of the Hardware and related configurations (i.e. fast disk, many cores...) might be needed. We expect, thanks to such activity, to perform cost/benefit analysis to judge the path forward. 
Longer term, based on the outcome of all these eventual activities, is then to propose a model to effectively manage pledges to support a "national-level" pool of resources for data analysis.

## Why

There are two main motivations that guide us in this activity. On one side we look at the HL-LHC era where we expect an order of magnitude increase of event rates which means a significantly increased dataset volumes. We expect this will impact not only on data management, meaning that not only that users cannot necessarily keep all their data locally on a laptop, but also con Workload Management. About the latter, we foresaw that the current bath-like model implemented over the GRID might be not enough to provide the necessary computing power for CMS analysis. Or better, it could not be effective in terms of time to insight (i.e. time to publication) and could become a limitation even from the perspectives of resource completion.

On the other side, many advancements in technologies wise arise in the last decade. This is true both regarding data analysis framework (and libraries) and also regarding the resource management (i.e. cloud applications). Different approaches coming from the industry might become/are becoming a source of inspirations for our domain. Applying industry standards nowadays available in big data analytics might help us to explore new lands. One obvious example in this respect is the trend in data exploration and analysis based on interactive model (or quasi interactive) that still might represent a huge paradigm shift with respect to the batch-like oriented approach we have been used since decades. This is not only about implementing a quick turnaround, but it's a key handle to open the doors to a different pattern in resources usage.

Note that concretely we don't foresee and revolution here, but rather a smooth transition toward a hybrid model that likely need to provide and support  both batch and interactive handles.

Last but not least, there is also a third aspect worth to be mentioned. Such a project allow building, again not resources wise, a national pool of resource that any physicist might like to access to run data analysis. From a user perspective, this is something like any other classical batch system.

## Technological Background and possible synergies

Most of the technology pillars of the project come from R&D developments made by INFN in several distinct initiatives developed during the past few years. To mention a few of them we have had [INDIGO-DataCloud](url), [EOSC-Hub](url), [ESCAPE](url) that provided us the foundations from cloud development to DataLake implementation and of course the solution for integrating data and compute. Resource integration experience gained with "PRACE access grant" has also a very important role because it gave us the concrete opportunity to test innovative solutions for accessing and using heterogeneous resource setups. A very significant aspect to highlight is that, all these developments are not experiment specific and thus the pillars are not tight to any specific computing model. There is a clear separation, at all levels, between experiment specific, possibly legacy, integration and the underlying system. This is a clear added-value.

Regarding the synergies there are plenty of opportunities, spanning from [INFN-Cloud](https://www.cloud.infn.it/) project to Multi-Experiment evaluation, passing through collaboration with initiatives for further integration of HPC and Cloud (i.e. the continuum) within ongoing initiatives with CINECA but also collaboration with analysis frameworks developers such as the current collaboration established with [ROOT RDF](url) team. 
Regarding the HPC it is about interactive data processing which thing become even more interesting when we start caring about integration ML based workflows.

## State of the Art

Discussions about possible strategies to build such facilities for analysis become nowadays a hot topic. Here we aim to provide a list of initiatives, within CMS Collaboration, ad different level of maturity, that are actively proposing and comparing solutions:

- Coffea Casa @ UNL
- AF @ MIT - Infrastructure
- AF @ Purdue Status and Plans
- AF at CIEMAT
- Elastic Analysis Facility @ FNAL
- SWAN @ CERN

## List of talks

 <span style="color:red;font-size: large;font-weight: bold;">add here all the presentation done. Perhaps list all the forum where this matter are discussed.</span>

- _"INFN AF DEMO: The VBS analysis example"_, CMS Week, 26-Jan-2022
- _"A prototype for interactive analysis at INFN"_, PPP Meeting, Nov-2021
- _"A prototype for interactive analysis at INFN"_, CMS ATTF Meeting, Oct-2021

