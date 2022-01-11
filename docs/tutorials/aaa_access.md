# AAA Access

Data stored in the tiers of the WLCG (i.e. grid) can be accessed by using a specific redirector: ```xrootd-cms.infn.it```.
Once you know the logical path of your file, you only need to add ```root://xrootd-cms.infn.it/``` at the beginning to that path in order to copy it using the ```xrdcp``` command or using it as a data source for a RDataFrame:

```
chain = ['root://xrootd-cms.infn.it//store/user/dummy-user/dummy-folder/file1.root', 'root://xrootd-cms.infn.it//store/user/dummy-user/dummy-folder/file2.root']
df = ROOT.RDataFrame(chain)
```
