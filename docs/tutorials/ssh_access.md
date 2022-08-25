# Accessing your UI via SSH

You'll be able to spawn and access your UI simply by following these instructions

- go to [https://cms-it-hub.cloud.cnaf.infn.it/hub/token](https://cms-it-hub.cloud.cnaf.infn.it/hub/token) and get a token. Take note of it.

- use the token as password to connect to your on-demand UI via:

```bash
ssh <username>@cms-it-hub.cloud.cnaf.infn.it -p 32022
```