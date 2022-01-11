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
# Use Dask interactively as a backend
Dask can be used as a backend for the distributed execution of ROOT's [RDataFrame](https://root.cern/doc/master/classROOT_1_1RDataFrame.html) computations. More specifically, ROOT RDataFrame supports distributed execution via the ```ROOT.RDF.Experimental.Distributed``` module.

## Set up
In a notebook, once a Dask client has been deployed and a Dask client instatiated:
```
from dask.distributed import Client
client = Client(<Dask scheduler address)
```

you need to declare a distributed DataFrame, passing all the necessary information
```
import ROOT
df = ROOT.RDF.Experimental.Distributed.Dask.RDataFrame(<name of the tree>, 
                                                       chain, 
                                                       npartitions=<maximum number of partitions (i.e. Dask tasks)>, 
                                                       daskclient=client)
```
In most cases you would like to declare custom functions to the ROOT interpreter in order to perform specific calculations on data. To do this, you need to use the ```ROOT.RDF.Experimental.Distributed.initialize``` function to initialize each worker.
```
text_file = open("postselection.h", "r")
data = text_file.read()
def my_initialization_function():
    ROOT.gInterpreter.Declare('{}'.format(data))
ROOT.RDF.Experimental.Distributed.initialize(my_initialization_function)
```
now you are ready to book all the computations you need to do on the dataframe using RDataFrame methods.
Once all the operations are booked, trigger the distributed execution by doing some actions on booked items, as you would do using RDataFrame locally. Results can be accessed in the same way as local RDataFrame, too.
