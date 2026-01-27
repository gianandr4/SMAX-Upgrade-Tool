---
config:
  namespace: itsma-clienta
  ESM_NAMESPACE: itsma-clienta
  version: '24.4'
  registry: registry.example.com
  registry_server: registry.example.com
  nfs_server: nfs.internal.com
  username: admin
  password: changeme
  image_secret_name: regcred
---

# Client A Upgrade to v24.4

## Preparation

### SMAX Health Check

Ensure all pods are in 2/2 or 1/1 Running state before starting.
- [x] Step completed

```bash
kubectl get pods -n {{namespace}}
```

#### Notes

Current version notes here

##### 24.4

Current version notes here

### - [ ] Complete

### Create Docker Registry Secret

Create secret for pulling images from private registry.
- [ ] Step not completed

```bash
kubectl get secret <image_secret_name> -n <ESM_NAMESPACE>
```

### - [ ] Complete

### External DB Backup

Backup BoB, SmartAnalytics, and IdM databases manually.
- [ ] Step not completed

```bash
pg_dump -h {{nfs_server}} -U postgres bo_db > smax_backup_$(date +%Y%m%d).sql
```

### - [ ] Complete

## Upgrade

### Monitor Upgrade Progress

Watch pods restart and come back online.
- [x] Step completed

```bash
kubectl get pods -n <ESM_NAMESPACE> -w
```

### - [ ] Complete

## Verification

### Verify SMAX Version

Confirm the upgrade was successful.
- [x] Step completed

```bash
kubectl get pods -n {{namespace}} -o jsonpath='{.items[0].spec.containers[0].image}'
```

### - [ ] Complete

### Run Smoke Tests

Test basic functionality: login, create ticket, check reports.
- [x] Step completed

### - [ ] Complete

### Test Checklist:

- [x] Step completed

### - [ ] Complete

