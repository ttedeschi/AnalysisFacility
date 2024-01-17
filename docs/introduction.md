<h1 style="margin-bottom: 0px">Building a testbed<br>for an Analysis Facility at INFN</h1>
<h5>a Prototype</h5>

## About

This is an R&D project that aims at building a testbed for an analysis facility at INFN. The testbed is intended for end-users (expert in data analysis) and developers in computing matters. As such it must be seen as a platform available for either exploiting new analysis framework or running/porting existing analysis, as well as exploiting new ideas for resources integration and provisioning. Some concrete examples are: 

1. __Comparing distinct approaches such as interactive vs batch like analysis__. From a user perspective this can be seen as the place where to test/benchmarking how a Jupyter notebook as big as a whole Tier2 can perform
2. __Validate and gain experience with new frameworks (RDF, Coffea etc)__. A place where easily testing examples of analysis code and how they look like when written in a declarative fashion.
3. __Study new solution for transparent resource integration.__ For a computing developer this is the place where we provide a concrete implementation for "continuum model" where Grid, Cloud and HPC coexist transparently for the user. 
4. __Experimenting and developing new solutions for resource provisioning__ From a computing expert point of view, here is where suggest what to experiment in order to overcome some known current GRID "limitations".

### Software stack

To this end, the prototype provides a user-friendly computational environment that pretends to simplify and facilitate the overall analysis process, enabling rapid processing of data eventually in a column-wise fashion. The technical services are based on __open source industry standards__. More in detail, they are based on Dask and Jupyter notebooks and rely on HTCondor for a transparent integration of dispersed (and heterogeneous) computing resources. It also guarantees resource allocation based on fair-share (got out of the box from HTCondor). The technical architecture is described [here](how_it_works/components.md).

### Hardware 

Hardware wise, we currently plan to be able to do __all the initial activities using a very limited fraction of the existing funded resources (mostly at Tier2).__ In addition, we plan to rely on a limited amount of computing power provided by INFN-Cloud (actually cloud@CNAF) that represents the bare minimum backbone for the AF where we are deploying the central services and where to develop/test with the user Jupyter notebook. The details about the currently available resources can be found [here](how_it_works/available_resources.md). 

### Objectives

The first objective of the project is not about purchasing any new hardware but rather about __to gain experience on new approaches to data analysis as well as to evolve the resource access strategies.__ The main scopes of this R&D process are the following:

- learn how to better exploit any available provider.
- understand changes in the type of the Hardware and related configurations (i.e. fast disk, many cores...) might be needed
- perform cost/benefit analysis to judge the path forward.
- (longer term) propose a model to effectively manage pledges to support a "national-level" pool of resources for data analysis.

## Motivations

> - __Addressing HL-LHC era challenges where we expect an order of magnitude increase of event rates__ which means a significantly increased dataset volumes. 
>    - impact not only on data management, meaning that not only that users cannot necessarily keep all their data locally on a laptop, but also con Workload Management
> - __Current batch-like model implemented over the GRID might be not enough__ to provide the necessary computing power for CMS analyses 
>    - it could not be effective in terms of time to insight (i.e. time to publication) and could become a limitation even from the perspectives of resource completion.


Many advancements in technologies arise from the last decade both regarding data analysis framework (and libraries) and also regarding the resource management (i.e. cloud applications). Different approaches coming from the industry might become/are becoming a source of inspirations for our domain. Applying big data analytics industry standards might help us to explore new lands. One obvious example in this respect is the trend in __data exploration and analysis based on interactive model__ (or quasi interactive). 

This still might __represent a huge paradigm shift__ with respect to the batch-like oriented approach we have been used since decades. This is not only about implementing a quick turnaround, but it's a key handle to open the door to a different pattern in resources usage.

> We don't foresee a revolution here, but rather __a smooth transition__ toward a hybrid model that likely need to provide and support both batch and interactive handles.

Last but not least, there is also a third aspect worth to be mentioned. Such a project allow building, again not resource wise, a national pool of resource that any physicist might want to access to run data analysis in a legacy batch fashion. From a user perspective, this is something like any other classical batch system.

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

## List of publications
- Tommaso Tedeschi, Vincenzo Eduardo Padulano, Daniele Spiga, Diego Ciangottini, Mirco Tracolli, Enric Tejedor Saavedra, Enrico Guiraud, Massimo Biasotto, _"Prototyping a ROOT-based distributed analysis workflow for HL-LHC: The CMS use case"_, Computer Physics Communications, Volume 295, 2024, 108965, ISSN 0010-4655, https://doi.org/10.1016/j.cpc.2023.108965.

## List of talks

 <span style="color:red;font-size: large;font-weight: bold;">add here all the presentation done. Perhaps list all the forum where this matter are discussed.</span>


- [_"CMS Tier2 integration in INFN Analysis Facility"_](https://indico.cern.ch/event/1328197/#4-cms-tier2-integration-in-inf), CMS CAT General Meeting, 11-Oct-2023
- [_"INFN Analysis Facility"_](https://indico.cern.ch/event/1274741/#15-infn-analysis-facility), CMS CAT General Meeting, 24-May-2023
- [_"Benchmarking distributed-RDataFrame with CMS analysis workflows on the INFN analysis infrastructure"_](https://indico.jlab.org/event/459/contributions/11593/), CHEP 2023, 9-May-2023
- _"INFN AF DEMO: The VBS analysis example"_, CMS Week, 26-Jan-2022
- _"A prototype for interactive analysis at INFN"_, PPP Meeting, Nov-2021
- _"A prototype for interactive analysis at INFN"_, CMS ATTF Meeting, Oct-2021

