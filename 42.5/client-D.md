---
config:
  namespace: itsma-clienta
  ESM_NAMESPACE: itsma-clienta
  version: '42.5'
  registry: registry.example.com
  registry_server: registry.example.com
  nfs_server: nfs.internal.com
  username: admin
  password: changeme
  image_secret_name: regcred
---

# Client A Upgrade to v42.5

## Preparation

### SMAX Health Check

Ensure all pods are in 2/2 or 1/1 Running state before starting.
---
---
---
---
#### History (from v24.4 - 2026-01-26)
![image.png](https://raw.githubusercontent.com/gianandr4/SMAX-Upgrade-Tool/main/24.4/images/1769447437446-image.png)
---

```bash
kubectl get pods -n {{namespace}}
```

- [ ] Step not completed

**Personal Notes:**


#### History (from v24.5 - 2026-01-26)
asda

---

### Create Docker Registry Secret

Create secret for pulling images from private registry.
---
---
---
---
---

```bash
kubectl get secret <image_secret_name> -n <ESM_NAMESPACE>
```

- [ ] Step not completed

**Personal Notes:**


#### History (from v24.5 - 2026-01-26)
#### History (from v24.4 - 2026-01-26)
Remember to verify the secret was created:

---

### External DB Backup

Backup BoB, SmartAnalytics, and IdM databases manually.
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

- [ ] Step not completed

---

### Test Checklist:

---
---
---
---
---

- [ ] Step not completed

---

