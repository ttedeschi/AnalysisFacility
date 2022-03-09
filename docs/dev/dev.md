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
# :information_source: General info

## :material-github: Github issue table 

Table of open issues: [https://github.com/orgs/comp-dev-cms-ita/projects/1](https://github.com/orgs/comp-dev-cms-ita/projects/1?fullscreen=true)

## :material-server-network: Proxy server

> TODO: create a repo for xrootd docker compose with doc: https://github.com/comp-dev-cms-ita/compose-xrootd

At the moment the demo server proxy is: `root://131.154.97.130.myip.cloud.infn.it:30443/`

## :material-application: Condor

Master k8s: root@131.154.96.124

get schedd pod name: `kubectl get pod -n af -l app.kubernetes.io/name==schedd`

and log into it.

From there all the condor commands are available

e.g. you can list the SiteName of a node with: `c`ondor_q -l nodename`

## :material-web: JupyterHub

pod del hub
`kubectl get pod -n af | grep hub`

pod dei pod utenti
`kubectl get pod -n af | grep jupyter`

__members of group /cms are users__

__members of group /cms/it are admins__

## :material-web: WebUI's

kubeapps: [https://kubeapps.131.154.96.124.myip.cloud.infn.it](https://kubeapps.131.154.96.124.myip.cloud.infn.it)

k8s dashboard: [https://dashboard.131.154.96.124.myip.cloud.infn.it](https://dashboard.131.154.96.124.myip.cloud.infn.it)

grafana: [https://grafana.131.154.96.124.myip.cloud.infn.it](https://grafana.131.154.96.124.myip.cloud.infn.it)

htc site guide: [https://comp-dev-cms-ita.github.io/compose-htc-wn](https://comp-dev-cms-ita.github.io/compose-htc-wn)