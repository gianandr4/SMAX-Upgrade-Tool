---
config:
  namespace: itsma-clienta
  ESM_NAMESPACE: itsma-clienta
  version: '24.5'
  registry: registry.example.com
  registry_server: registry.example.com
  nfs_server: nfs.internal.com
  username: admin
  password: changeme
  image_secret_name: regcred
---

# Client A Upgrade to v24.5

## Preparation

### SMAX Health Check

Ensure all pods are in 2/2 or 1/1 Running state before starting.

```bash
kubectl get pods -n {{namespace}}
```

- [ ] Step not completed

#### Notes

##### Current



##### 24.4



---

**📚 Previous Version Notes (v24.4 - 2026-01-27)**

#### Notes

##### Current

---

### Create Docker Registry Secret

Create secret for pulling images from private registry.
---
---
---
---
---
---
---
---
---
---
---

```bash
kubectl get secret <image_secret_name> -n <ESM_NAMESPACE>
```

- [ ] Step not completed

---

### External DB Backup

Backup BoB, SmartAnalytics, and IdM databases manually.
---
---
---
---
---
---
---
---
---
---
---
---

```bash
pg_dump -h {{nfs_server}} -U postgres bo_db > smax_backup_$(date +%Y%m%d).sql
```

- [ ] Step not completed

---

## Upgrade

### Monitor Upgrade Progress

Watch pods restart and come back online.
---
---
---
---
---
---
---
---
---
---
---
---

```bash
kubectl get pods -n <ESM_NAMESPACE> -w
```

- [ ] Step not completed

---

## Verification

### Verify SMAX Version

Confirm the upgrade was successful.
---
---
---
---
---
---
---
---
---
---
---
---

```bash
kubectl get pods -n {{namespace}} -o jsonpath='{.items[0].spec.containers[0].image}'
```

- [ ] Step not completed

---

### Run Smoke Tests

Test basic functionality: login, create ticket, check reports.
---
---
---
---
---
---
---
---
---
---
---

- [ ] Step not completed

---

### Test Checklist:

---
---
---
---
---
---
---
---
---
---
---
---

- [ ] Step not completed

---

