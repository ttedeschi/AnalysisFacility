# Resource Available 

The current approach is to provide a small amount of resources provided by INFN-Cloud (namely the _Cloud@CNAF_ provider) plus compute node and data cache available on distributed clusters.

<span style="color:red;font-size: large;font-weight: bold;">ESPLICITARE NNN e MMM</span>

## Central Serivces and resources @ CNAF

 This is currently hosting the follwing service: 

- JupyterHub
- k8s
- ...

The amount of resources are
- NNN cores
- MMM storage
Out of which KKKK are used as locally available computing power for data analysis

## Resources at Tier2

At that stage of the project the bulk of the compute and storage resources are provided by Legnaro's Tier2 although all the rest of the CMS-Italy Tier2 federation has validated the system with a basic functional test. Moreover, nothing prevent to extend the very same integration to our Tier1 at CNAF. Technically there are no limitation, the choice has been to start with Tier2 because of the role of the Tier2 vs Tier1 as defined by the Computing Model of CMS.

### Legnaro

Legnaro deploys both compute and data service in form of worker nodes and data cache. The amount of currently available resources are

- NNN cores
- MMM TB of cache

## Opportunistic resources

One of the peculiarity of the developed system is to allow a seamless integration of any opportunistic resources. There are currently two distinct integrations that differ from several perspectives:

- Type of Hardware: Architecture ( x86 vs PowerPC ). Accelerators CPU only, CPU plus GPU
- Scale:  
- Type of setup/provider: HPC vs Cloud.

### CloudVeneto @ Padova

CloudVeneto provides resources for both compute and data. The current amount of resources available are:

- NNN cores
- MMM storage

### Cineca M100

This is the less mature integration and the most challenging from the technical perspectives. Network segregation, unconventional architecture and the presence of accelerators on the node make this a system quite different with respect to the majority of the systems we are used to. The M100 integration has been done with the aim to offer the user the possibility to transparently access a whole node and execute a JupyterLab over there.
