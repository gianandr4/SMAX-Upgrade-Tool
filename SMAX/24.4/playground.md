---
config:
  client: Playground
  ESM_NAMESPACE: itsma-clienta
  version: '24.4'
  installation_path: /opt/cdf
  omt_binaries_path: /
  CDF_HOME: /
  namespace: itsma-clienta
  nfs_server: nfs.internal.com
  username: admin
  password: changeme
  image_secret_name: regcred
---

# Playground Upgrade to v24.4

## OMT Prerequisite tasks

### SMAX Health Check

Ensure all pods are in 2/2 or 1/1 Running state before starting.

```bash
kubectl get pods -n {{namespace}}
```

### - [x] Complete

