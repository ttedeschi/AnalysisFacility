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

##Distributing your code to the workder nodes
If your job needs code and libraries, e.g. your analysis framework, and even small files, to be available on the worker node (i.e. you need to achieve what you would probably do in lxplus by a simple ```cd``` in the relevant directory and a source of the environment, e.g. with ```cmsenv```), you can leverage ```docker```, ```singularity``` and ```cvmfs```. 

Scared :fearful:? Don't be. 

For the time being just know that you can instruct the job to pick an image that holds your software with one line added to your HTCondor submission file:

```
+SingularityImage = "/path/to/your/image/in/cvmfs/"
Requirements   = (SiteName == "T2_LNL_PD")
```

!!! Fixme
     The second lime is there only for the time being, listing the only site that has all features implemented. But this is a test and will be propagated to other sites soon (August 30th 2022).

And there is an advantage: **your image is a permanent snapshot of the exact code that you are going to use in your job**. This is very handy for version management.

## Creating an image of your code: the Latinos code example

Please follow the instructions [here](../user_defined_image.md) to get started with using your own image.
