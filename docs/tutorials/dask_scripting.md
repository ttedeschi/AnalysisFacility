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

# :fontawesome-brands-python: Use Dask from Python

## Managing DASK clusters via CLI

It is possible to create a Dask Cluster using directly a Python script.

## Requirements

- go to https://cms-it-hub.cloud.cnaf.infn.it/hub/token and get a token. Take note of it.

- use the token as password to connect to your on-demand UI via:

```bash
ssh <username>@cms-it-hub.cloud.cnaf.infn.it -p 32022
```

## Install dfleel CLI

- Install via pip
```bash
pip install git+https://github.com/comp-dev-cms-ita/dfleet
```

- Setup the environment

```bash
# you do NOT need to set JUPYTERHUB_API_TOKEN  if you sit on a jlab instance
# this is only needed if you are running the CLI from your laptop
export JUPYTERHUB_API_TOKEN=<PUT JHUB TOKEN HERE>


export JUPYTERHUB_HOST=https://cms-it-hub.cloud.cnaf.infn.it
```

> For more information on how to use dfleet please see [here](https://github.com/comp-dev-cms-ita/dfleet/blob/main/README.md)

## Create a cluster

You can create a predefined cluster via:

```bash
dfleet  cluster create
```
If everything went well, you will be eventually prompted with a valid scheduler url and a link to the DASK dashboard.


> For more advanced creation options you can type:
> ```bash
> dfleet  cluster create --help
> ```

## :gear: Set up

Now you are ready to start using the dask cluster via python script. You just need to initialize the DASK client with:

```python
from dask.distributed import Client

client = Client(<Dask scheduler address)
```

You are now ready to go and to distribute your code!

When done, you can turn off your cluster via:

```bash
dfleet cluster delete <PUT CLUSTER ID HERE>
```

## Script Example

You can see here a full script example that you can use to test the setup:

```python
import os
from time import sleep

import dask.array as da
from dask_remote_jobqueue import RemoteHTCondor
from distributed import Client


def main():

    # Connect the client to the cluster scheduler
    client = Client(cluster.scheduler_address)
    
    # Run a command on workers
    print(client.run(os.getpid))

    def inc(x: int) -> int:
        return x + 1

    # Submit a function
    future = client.submit(inc, 10)
    print(future.result())

    # Compute a matrix multiplication
    x = da.random.random((100, 100))
    y = x + x.T
    print(y.compute())


if __name__ == "__main__":
    main()
```

## Using RDataFrame

If you want to use the DASK cluster within your RDataFrame code, youjust need to declare a distributed DataFrame, passing all the necessary information, like:

```python
import ROOT

df = ROOT.RDF.Experimental.Distributed.Dask.RDataFrame(
    <name of the tree>, 
    chain, 
    npartitions=<maximum number of partitions (i.e. Dask tasks)>, 
    daskclient=client,
)
```

In most cases you would like to declare custom functions to the ROOT interpreter in order to perform specific calculations on data. To do this, you need to use the ```ROOT.RDF.Experimental.Distributed.initialize``` function to initialize each worker.

```python
text_file = open("postselection.h", "r")
data = text_file.read()

def my_initialization_function():
    ROOT.gInterpreter.Declare('{}'.format(data))

ROOT.RDF.Experimental.Distributed.initialize(my_initialization_function)
```

now you are ready to book all the computations you need to do on the dataframe using RDataFrame methods.
Once all the operations are booked, trigger the distributed execution by doing some actions on booked items, as you would do using RDataFrame locally. Results can be accessed in the same way as local RDataFrame, too.

