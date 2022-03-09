# Report an issue

If you encounter any problem related to the extension or the cluster management, you can
open an issue [here](https://github.com/comp-dev-cms-ita/dask-remote-jobqueue/issues).

Remember to send information about the current environment, for example the current installed packages:

```bash
# Dask package versions
pip freeze | grep dask

# scipy package versions
pip freeze | grep scipy
```

Also, it could be useful to report the error if it is present in the the _labextension_ **logs**. The corresponding path is: `/var/log/dask_labextension.log`.
