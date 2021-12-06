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

# Hot to start the cluster

To create a Dask cluster from Jupyter Lab you have to use the Dask Lab Extension.
You can find it on the left sidebar as shown in the figure:

![dask labextension](imgs/dask_init_labextension.png)

Then, you can click on the `NEW` button of the Dask Lab Extension:

![dask new](imgs/dask_init_new.png)

You must select a proper factory name from the available list and, once selected,
you can press the `Create` button:

![dask select](imgs/dask_init_select.png)

After a couple of minutes, you will see the created cluster in the cluster list,
indicated in the image by the red square. If you select that cluster, you will see
also a list of available fields to monitor and inspect the cluster, the blue square
in the image. The Dask Lab Extension will select automatically for you the first
cluster created, as a consequence, you should see also all the fields available in
the dashboard (blue square).

![dask cluster created](imgs/dask_init_created.png)

> **NOTE**: in the `Scheduler Address` field you will see the actual status of the
> cluster creation. The cluster will be ready when there is present a valid
> cluster address such as `localhost:12345`.

Now, you can connect to the cluster using a `Dask Client`. Open your notebook,
select the cell where you want to put the client connection code, and click
the `Inject Code` button (red circle). Then, you will see the code in your cell,
as shown in the following image with the green square:

![dask cluster connect](imgs/dask_client_code.png)

> **NOTE**: you can also write that code copying the proper scheduler address
> shown in the cluster `Scheduler Address`, for example:
> 
> ```python
> from dask.distributed import Client
> 
> client = Client("localhost:12345")
> ```

# Cluster Management
## Scale

All new clusters will have `0` workers, thus, you need to scale your Dask cluster
to make a calculus. To scale the cluster, you can use the appropriate button:

![dask scale button](imgs/dask_scale_button.png)

A dialog screen will be shown. There, you can update the number of workers with
the desired value and then press the `Scale` button.

![dask scale modal](imgs/dask_scale_modal.png)

## Check resources

After a scale, the resource availability of the cluster will be automatically updated.
The information will be available in the cluster list, as shown in the following image:

![dask cluster resources](imgs/dask_management_resources.png)

However, the supply of resources is not instantaneous. As a consequence, you can
check the resources actually attached to the cluster using the `workers` item from
the dashboard list:

![dask workers](imgs/dask_management_workers.png)

A new tab will open, with some information about the current attached workers.
As soon as the workers come up, you will see the list populating with new workers,
with a situation similar to the following (where there are two workers):

![dask workers tab](imgs/dask_management_workers_tab.png)

## Delete the cluster

To remove the cluster, simply click on the `Shutdown` button. The cluster and its
workers will be deleted automatically.

![dask delete cluster](imgs/dask_delete_cluster.png)