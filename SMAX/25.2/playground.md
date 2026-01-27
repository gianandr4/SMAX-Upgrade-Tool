---
config:
  client: Playground
  ESM_NAMESPACE: itsma-clienta
  version: '24.4'
  installation_path: /opt/cdf
  omt_binaries_path: /home/omtbina
  CDF_HOME: /
  namespace: itsma-clienta
  username: ptadmin
---

# Playground Upgrade to v24.4

## OMT Prerequisite tasks

### AutoUpgrade 

If you'll perform an automated upgrade of OMT as a regular user, for example, you can run ./autoUpgrade -n 192.0.2.0 -u <username>.

```bash
.<omt_binaries_path>/autoUpgrade -n 192.0.2.0 -u <username>
```

### - [ ] Complete

