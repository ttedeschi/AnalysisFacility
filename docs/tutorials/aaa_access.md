# :material-database: AAA Access

Data stored in the tiers of the WLCG (i.e. grid) can be accessed by using a specific redirector: `xrootd-cms.infn.it`.
Once you know the logical path of your file, you only need to add `root://xrootd-cms.infn.it/` at the beginning to that path in order to copy it using the `xrdcp` command or using it as a data source for a RDataFrame:

```python
chain = [
    'root://xrootd-cms.infn.it//store/user/dummy-user/dummy-folder/file1.root', 
    'root://xrootd-cms.infn.it//store/user/dummy-user/dummy-folder/file2.root',

]
df = ROOT.RDataFrame(chain)
```

## Reading files via proxy
Moreover, files can also be read directly from a site using a proxy. In this case, further configuration is needed. 
If you want to read them from the JupyterLab instance, you only have to set these two environmental variables:
```
export X509_USER_PROXY=<path_to_your_proxyfile>
export X509_CERT_DIR=/cvmfs/grid.cern.ch/etc/grid-security/certificates/
```

Instead, if you want to use distributed Daskz, you need to properly configure each Dask worker:
- First of all, you need your proxyfile to be trasferred to each worker. This can be done via a Dask Plugin: 
  ```python
  from dask.distributed import Client
  from distributed.diagnostics.plugin import UploadFile
  
  client = Client(address="tcp://127.0.0.1:"+str(sched_port))
  client.register_worker_plugin(UploadFile("<path_to_your_proxyfile>"))
  ```
  The file will be uploaded in the working directory of each Dask process.
- Then, define in each Dask process all the necessary environmental variables for authentication:
  ```python
  def set_proxy(dask_worker):
      import os
      import shutil
      working_dir = dask_worker.local_directory
      proxy_name = 'proxy'
      os.environ['X509_USER_PROXY'] = working_dir + '/' + proxy_name
      os.environ['X509_CERT_DIR']="/cvmfs/grid.cern.ch/etc/grid-security/certificates/"
      return os.environ.get("X509_USER_PROXY"), os.environ.get("X509_CERT_DIR") 

  client.run(set_proxy)
  ```

## Moving files via Rucio
You can also replicate data on the grid into a desired site, as long as you have the necessary permissions and available storage.
This can be done via Rucio:
```python
from rucio.client.client import Client
import os
os.environ["RUCIO_HOME"] = "/cvmfs/cms.cern.ch/rucio/current/"
os.environ['X509_CERT_DIR'] = "/cvmfs/grid.cern.ch/etc/grid-security/certificates/"
os.environ['X509_USER_PROXY'] = "<path_to_your_proxyfile>"

RUCIO_SCOPE = "cms"
ACCOUNT = "<username>" #e.g. "ttedesch"
SITENAME = "<site>" #e.g. "T2_IT_Legnaro"
c = Client(account=ACCOUNT, auth_type="x509_proxy")

CMS_DATASET_NAME = "<dataset_name>"  #e.g. "/ZZTo4L_M-1toInf_TuneCP5_13TeV_powheg_pythia8/RunIISummer20UL17NanoAODv9-106X_mc2017_realistic_v9-v1/NANOAODSIM"
```

To get file replicas at a desired site:
```python
files = c.list_files(scope=RUCIO_SCOPE, name=CMS_DATASET_NAME)
lfns = []
for f in files:
    lfns.append(RUCIO_SCOPE + ':' + f['name'])
pfns_dicts = c.lfns2pfns(SITENAME, lfns, scheme='davs', operation='read')
pfns = []
for _,i in pfns_dicts.items():
    pfns.append(i)
print(len(pfns))
```

To replicate a dataset in a desired site:
```python
did = {'scope': RUCIO_SCOPE, 
       'name': CMS_DATASET_NAME,
       'type': 'DATASET'}

c.add_replication_rule([did], 1, SITENAME, lifetime=15778800)
```
You can also replicate per block:
```python
blocks_gen = c.list_content(scope=RUCIO_SCOPE, name=CMS_DATASET_NAME)
blocks = []
for bl in blocks_gen:
    blocks.append(bl)
c.add_replication_rule(blocks, 1, SITENAME, lifetime=15778800)
```
You can monitor the status of the replica via CLI:
```
 rucio list-rules cms:<dataset_name> #e.g. cms:/VBS_SSWW_TT_polarization_TuneCP5_13TeV-madgraph-pythia8/RunIISummer20UL17NanoAODv9-106X_mc2017_realistic_v9-v1/NANOAODSIM
```
