---
config:
  client: Playground
  ESM_NAMESPACE: itsma-clienta
  version: '24.4'
  installation_path: /opt/cdf
  omt_binaries_path: /home/omtbinaa
  CDF_HOME: /
  namespace: itsma-clienta
  username: ptadmin
---

# Playground Upgrade to v24.4

## OMT Prerequisite tasks

### Snapshots

- [ ] Down smax 
- [ ] Down core 
- [ ] Kubestop.sh 
- [ ] Οταν κατεβει το cluster – shutdown 
- [ ] Suspend ucmdb machines 
- [ ] Snapshots (No memory) 
- [ ] Turn on 
- [ ] Up core 
- [ ] Up smax 
- [ ] Kube-start.sh on workers 

### - [ ] Complete

### AutoUpgrade 

If you'll perform an automated upgrade of OMT as a regular user, for example, you can run ./autoUpgrade -n 192.0.2.0 -u <username>.

```bash
.<omt_binaries_path>/autoUpgrade -n 192.0.2.0 -u <username>
```

### - [ ] Complete

