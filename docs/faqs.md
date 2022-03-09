# :material-frequently-asked-questions: Frequently Asked Questions

## :material-chat-question: General

#### 1. How can I report an issue?

Just follows the istrucions in the [issue topic](tutorials/issues.md).

#### 2. How can I suggest an enhancement?

If you want a new feature to be supported, you can
open an issue [here](https://github.com/comp-dev-cms-ita/dask-remote-jobqueue/issues).

## :material-web: JupyterLab

### Tasks and running jobs

Here you can find questions and aswers about the interactive session. In particular,
concerning specific situation after the analysis is launched and you are waiting
for the results in your notebook or terminal.

#### 1. May I close the browser tab during an interactive session?

If a Dask job is running it is necessary to leave the tab and the browser open to collect
the results of the operation.

#### 2. May I logout during an interactive session?

This presents the same situation of Question `#1`

## :material-engine: Dask

### :material-server: Cluster

In this section there are questions about the Dask framework and cluster management.
#### 1. Is it possible to change the scheduler or the dask worker flavor?

With the image version `v1.0.1` is it not possible to change the default flavor of the
scheduler and the worker jobs directly from the UI.
