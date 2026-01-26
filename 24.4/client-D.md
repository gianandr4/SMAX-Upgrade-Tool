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

```bash
kubectl get pods -n {{namespace}}
```

- [ ] Step not completed

---

### Create Docker Registry Secret

Create secret for pulling images from private registry.

```bash
kubectl create secret docker-registry <image_secret_name> --docker-username=<username> --docker-password=<password> --docker-server=<registry_server> -n <ESM_NAMESPACE>
```

- [ ] Step not completed

**Personal Notes:**
Remember to verify the secret was created:
```
kubectl get secret <image_secret_name> -n <ESM_NAMESPACE>
```

---

### External DB Backup

Backup BoB, SmartAnalytics, and IdM databases manually.

```bash
pg_dump -h {{nfs_server}} -U postgres bo_db > smax_backup_$(date +%Y%m%d).sql
```

- [ ] Step not completed

---

## Upgrade

#### Apply Suite Upgrade

This triggers the actual version change in OMT.

```bash
./upgrade.sh -n {{namespace}} -v {{version}}
```

- [ ] Step not completed

**Personal Notes:**
- Scheduled for January 20, 2024 at 2:00 AM
- Expected duration: 2-3 hours
- Keep backup ready for rollback

---

### Monitor Upgrade Progress

Watch pods restart and come back online.

```bash
kubectl get pods -n <ESM_NAMESPACE> -w
```

- [ ] Step not completed

---

## Verification

### Verify SMAX Version

Confirm the upgrade was successful.

```bash
kubectl get pods -n {{namespace}} -o jsonpath='{.items[0].spec.containers[0].image}'
```

- [ ] Step not completed

---

### Run Smoke Tests

Test basic functionality: login, create ticket, check reports.

- [ ] Step not completed

**Personal Notes:**
### Test Checklist:
- [ ] Login with test user
- [ ] Create incident ticket
- [ ] Verify notifications
- [ ] Check dashboard widgets
- [ ] Run sample report

---
