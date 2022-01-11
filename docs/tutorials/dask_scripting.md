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

# Use Dask from Python

## How to

It is possible to create a Dask Cluster using directly a Python script. To start,
you have to import the proper class and create the related object. The following example
is for a remote cluster on HTCondor Batch System:

```python
from dask_remote_jobqueue import RemoteHTCondor

cluster = RemoteHTCondor(ssh_namespace="af", sitename="Perugia", asynchronous=False)
```

At this point, the cluster is not yet running, and you have prepared the object.
Be aware that you have to indicate all the argument specified above:

* `ssh_namespace`: the namespace for the connection (it should be "af")
* `sitename`: where you want to go. If not specified, the Batch System will choose for you
* `asynchronous`: indicates that you want to use the Python script directly, thus it should be equal to `False`

To start the cluster you have to use the `start` method:

```python
cluster.start()
```

After that, you will have the Dask scheduler up and running. Now, you can scale the cluster manually:

```python
cluster.scale(1)
```

> **NOTE**: the `scale` method will not wait for the workers to come up, thus, it is suggested to
> scale immediately after the cluster starts and put some waits if necessary, to let the Batch
> System provides for the resources:
>
> ```python
> cluster.scale(1)
> sleep(60)
> ```
>
> **NOTE**: it could be possible that the cluster controller is not yet reachable when you
> try to `scale` your cluster. In this situation you could receive an exception like the following:
>
> ```python
> /usr/local/[...]/dask_remote_jobqueue.py in scale(self, n)
>     594 
>     595         if not connected:
> --> 596             raise Exception("Cluster is not reachable...")
>     597 
>     598         logger.debug("[Scheduler][scale][connection OK!]")
>
> Exception: Cluster is not reachable...
> ```
>
> To fix this situation, just add a small sleep before the scale too, like follows:
>
> ```python
> sleep(6) # Wait a bit before the scale
> cluster.scale(1)
> sleep(60)
> ```

Then, you can connect to the cluster with the Dask Client as follows:

```python
from distributed import Client

client = Client(cluster.scheduler_address)
```

From now on, you can use the Dask Client and the relatives libraries. At the end,
you can simply close the client and the cluster to conclude the script:

```python
client.close()
cluster.close()
```

> **NOTE**: the commands seen above can be used also in an interactive environment
> such as `iPython`.

## Script Example

You can see here a full script example:

```python
import os
from time import sleep

import dask.array as da
from dask_remote_jobqueue import RemoteHTCondor
from distributed import Client


def main():
    # Create the cluster object properly configured
    cluster = RemoteHTCondor(ssh_namespace="af", sitename="Perugia", asynchronous=False)
    # Start the cluster
    cluster.start()

    print(cluster.scheduler_address)
    print("Scale to 1 worker")

    # Scale the cluster
    cluster.scale(1)
    sleep(120)

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

    # Exit
    client.close()
    cluster.close()


if __name__ == "__main__":
    main()
```
