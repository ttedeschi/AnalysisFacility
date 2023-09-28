In priciple an Analaysis Facility user would like to use the AF infrastructure for its CI/CD tests, or whatever the user needs to trigger from its own machine.
In order to do this, it possible to access the HTCondor pool and even deploy and use a remote Dask Cluster on top of it without accessing the JupyterLab instance.
First of all, you need to start at least from an image which contains HTCondor. For simplicity purposes, let's start from the AF JupyterLab image:
```bash
docker run -ti ghcr.io/comp-dev-cms-ita/jupyterlab:v2.0.1-patch10-dask bash 
```
once you are there, the setup of the htcondor pool needs:
```bash
export _condor_SCHEDD_NAME=131.154.96.124.myip.cloud.infn.it
export _condor_SCHEDD_HOST=131.154.96.124.myip.cloud.infn.it
export _condor_COLLECTOR_HOST=131.154.96.124.myip.cloud.infn.it:30618
export _condor_SCITOKENS_FILE=<path to file containing the token stored in /tmp/token inside your Jlab instance>
export _condor_AUTH_SSL_CLIENT_CAFILE=/ca.crt
export _condor_SEC_DEFAULT_AUTHENTICATION_METHODS=SCITOKENS
export _condor_TOOL_DEBUG=D_FULLDEBUG,D_SECURITY
```
Once done this, the cluster should be accessible.

If you need to deploy a Dask Cluster:
```bash
export JUPYTERHUB_API_TOKEN=<copy from Jlab instance>
export REFRESH_TOKEN=<copy from Jlab instance>
export IAM_SERVER=https://cms-auth.web.cern.ch/
export IAM_CLIENT_ID=<copy from Jlab instance>
export IAM_CLIENT_SECRET=<copy from Jlab instance>
```
then, run:
```bash
git clone https://github.com/comp-dev-cms-ita/dask-remote-jobqueue.git -b standalone
cd dask-remote-jobqueue
pip install -e .
```
and finally, run via Python:
```python
from dask_remote_jobqueue import RemoteHTCondor
import time
cluster = RemoteHTCondor(
        user = "ttedesch", #substitute with your username
        ssh_url = "cms-it-hub.cloud.cnaf.infn.it",
        ssh_url_port = 31023,
        sitename = "T2_LNL_PD_CloudVeneto", #substitute with desired side
        singularity_wn_image = "/cvmfs/images.dodas.infn.it/registry.hub.docker.com/dodasts/root-in-docker:ubuntu22-kernel-v1", #substitute with your image
        asynchronous = False,
        debug = False
)
cluster.start()
```
Info about the Dask scheduler will be prompted when it goes into running state, including the Dashboad URL: it can be accessed on the localhost of your docker container. 
