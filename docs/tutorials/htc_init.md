<!--
 Copyright 2021 dciangot
 
 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at
 
     http://www.apache.org/licenses/LICENSE-2.0
 
 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
-->
#The Analysis Facility as a normal batch cluster
As mentioned in the introduction, the Analysis Facility can be used in a similar way as you might be familiar with at lxplus/lxbatch, i.e. as an HTcondor batch system.
The caveat is that the resources it uses are distributed in different Italian sites, so some care must be taken into ensuring that the same software environment is available on the jupiter hub and on the worker nodes.

At lxplus/lxbatch this is usually ensured leveraging ```afs```: the worker nodes mount ```afs```, and typically the user job goes into the relevant ```afs``` directory to source the environment, or find input files and software that is then executed on the worker node.

**This workflow cannot be implemented as is in the AnalysisFacility**, but one that is similar, and possibly better when it comes to version management, is possible.

!!! Fixme 
    July 4th 2022. The following assumes features that are available currently only on the [testbed JupyterHub](https://jhub-testbed.131.154.96.124.myip.cloud.infn.it/), using image ```dciangot/jlab-rclone:v7```, or private images derived from this one.

##Shared Home

A shared home is mounted on both the Jupyter Hub and on the worker nodes under ```/shared-home``` using [RClone](https://rclone.org/). It should be used to hold any user data that need to be accessed by the job at runtime, e.g. configuration files. 
!!! Attention
     The shared home has limitations with respect to ```afs```, e.g. *it is not possible to successfully create a CMSSW developer area*. 

!!! Attention
     The shared home is read-only on the worker nodes. 

##Distributing your code to the workder nodes
If your job needs code and libraries, e.g. your analysis framework, to be available on the worker node (i.e. you need to achieve what you would probably do in lxplus by a simple ```cd``` in the relevant directory and a source of the environment, e.g. with ```cmsenv```), you can leverage ```docker```, ```singularity``` and ```cvmfs```. 

Scared :fearful:? Don't be. 

For the time being just know that you can instruct the job to pick an image that holds your software with one line added to your HTCondor submission file:

```
+SingularityImage = "/path/to/your/image/in/cvmfs/"
Requirements   = (SiteName == "T3_IT_Perugia-test")
```

!!! Fixme
     The second lime is there only for the time being, listing the only site that has all features implemented. But this is a test and will be propagated to other sites soon (July 4th 2022).

And there is an advantage: **your image is a permanent snapshot of the exact code that you are going to use in your job**. This is very handy for version management.

## Creating an image of your code: the Latinos code example
This section describes the creation of a ```docker``` image with Latinos software. Latinos is a software stack to allow analysis of CMS data in final states with at least one lepton. It is a legacy framework, based on "traditional ROOT". The code resides [here](https://github.com/latinos/LatinoAnalysis). What is described in the following is just one way of achieving the goal of building the image, probably one of the most straightforward and best documented. It is entirely based on the instructions [here](https://awesome-workshop.github.io/docker-cms/03-cvmfs_containers/index.html).

### Placing the software on gitlab to use CI/CD pipelines
A CI/CD (Continuous Integration/Continuous Deployment) pipeline is a ```gitlab``` funtionality which allows defining a set of steps that need to be run following a commit or a merge request. We will use it to create the relevant ```docker``` image.
As a first step the Latinos repository was cloned on [```gitlab.cern.ch```](https://gitlab.cern.ch/lenzip/LatinoAnalysis), to be able to use CI/CD pipelines. A few additions were necessary:

   * a [```.gitlab-ci.yml```](https://gitlab.cern.ch/lenzip/LatinoAnalysis/-/blob/master/.gitlab-ci.yml) file instructing the creation of a pipeline to build the software. In essence, on every new commit, gitlab runs the pipeline, which, if succesful, results in the creation of a ```docker``` image of the latinos software stack. One of the most relevant lines of the pipeline configuration is the specification of the starting image, i.e. the one on top of which the software is built. In this case it was the already mentioned ```dciangot/jlab-rclone:v7```, so that the resulting image has the same functionalities of the original image, especially the capability to handle the Analysis Facility HTCondor cluster.
   * a [```.gitlab/SetupDocker.sh```](https://gitlab.cern.ch/lenzip/LatinoAnalysis/-/blob/master/.gitlab/SetupDocker.sh) holding the instructions to build the software. In this case the software is built in a ```CMSSW_11_2_5``` developer area inside ```/opt```. The job on the worker node will need to ```cd /opt/CMSSW_11_2_5``` and issue ```cmsenv```.

After every commit the pipeline is triggered and the ```docker``` image ends up [here](https://gitlab.cern.ch/lenzip/LatinoAnalysis/container_registry).

### Propagating the image to /cvmfs

Adding the images to ```/cvmfs``` is easy, using ```unpacked.cern.ch```, following instructions [here](https://awesome-workshop.github.io/docker-cms/06-unpacked/index.html), which ultimately boil down to make a merge request like [this one](https://gitlab.cern.ch/unpacked/sync/-/merge_requests/135).
Every night the images are propagated.

