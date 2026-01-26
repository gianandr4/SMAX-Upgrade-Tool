---
config:
  namespace: itsma-cliente
  ESM_NAMESPACE: itsma-cliente
  version: '24.4'
  registry: registry.example.com
  registry_server: registry.example.com
  nfs_server: nfs.internal.com
  username: admin
  password: changeme
  image_secret_name: regcred
  backup_location: /mnt/backup
---

# CLIENT E - SMAX Upgrade 24.4

## Pre-Upgrade Planning

### Review Release Notes

Review the official SMAX release notes to understand new features and changes.

- [ ] Step not completed

---

### Schedule Maintenance Window

Coordinate with stakeholders to schedule an appropriate maintenance window.

- [ ] Step not completed

**Personal Notes:**
- Proposed window: Saturday, 2:00 AM - 6:00 AM
- Stakeholders notified via email
- Change ticket: CHG00012345

---

## Binaries Download

### Download SMAX Binaries

Download the SMAX 24.4 installation binaries.

```bash
wget https://downloads.microfocus.com/smax/24.4/smax-24.4.tar.gz
tar -xzf smax-24.4.tar.gz
```

- [ ] Step not completed

---

### Upload to Registry

Upload images to the private registry.

```bash
./upload-images.sh -r {{registry_server}} -u {{username}} -p {{password}}
```

- [ ] Step not completed

---

## Pre-Upgrade Backup

### Backup PostgreSQL Databases

Create full backups of all SMAX databases.

```bash
pg_dump -h {{nfs_server}} -U postgres bo_db > {{backup_location}}/smax_backup_$(date +%Y%m%d).sql
```

- [ ] Step not completed

**Personal Notes:**
Backup completed successfully
- BoB database: 2.3 GB
- SmartAnalytics database: 1.8 GB
- IdM database: 450 MB
- Total backup size: 4.5 GB
- Backup location verified and accessible

---

### Backup Kubernetes Configurations

Export all Kubernetes configurations.

```bash
kubectl get all -n {{namespace}} -o yaml > {{backup_location}}/k8s-backup-$(date +%Y%m%d).yaml
```

- [ ] Step not completed

---

## Pre-Upgrade Validation

### SMAX Health Check

Ensure all pods are running before starting.

```bash
kubectl get pods -n {{namespace}}
```

- [ ] Step not completed

---

### Verify Storage

Ensure sufficient storage is available.

```bash
kubectl get pvc -n {{namespace}}
df -h
```

- [ ] Step not completed

---

### Check Resource Utilization

Review current CPU and memory utilization.

```bash
kubectl top nodes
kubectl top pods -n {{namespace}}
```

- [ ] Step not completed

**Personal Notes:**
Current resource usage looks good:
- CPU utilization: 45% average across nodes
- Memory utilization: 62% average
- Sufficient capacity for upgrade

---

## Registry Configuration

### Create Docker Registry Secret

Create secret for pulling images.

```bash
kubectl create secret docker-registry <image_secret_name> \
  --docker-username=<username> \
  --docker-password=<password> \
  --docker-server=<registry_server> \
  -n <ESM_NAMESPACE>
```

- [ ] Step not completed

---

### Verify Registry Access

Test registry connectivity.

```bash
kubectl run test-registry --image=<registry_server>/smax/sample:24.4 -n {{namespace}}
kubectl delete pod test-registry -n {{namespace}}
```

- [ ] Step not completed

---

## Upgrade Execution

### Place in Maintenance Mode

Enable maintenance mode to prevent user access.

- [ ] Step not completed

**Personal Notes:**
Maintenance page displayed to users
- Message: "System upgrade in progress"
- Estimated completion: 4 hours

---

### Apply SMAX Upgrade

Execute the main upgrade process.

```bash
./upgrade.sh -n {{namespace}} -v {{version}}
```

- [ ] Step not completed

---

### Monitor Upgrade Progress

Monitor the upgrade and watch for errors.

```bash
kubectl get pods -n {{namespace}} -w
```

- [ ] Step not completed

**Personal Notes:**
Upgrade progress tracking:
- Start time: 02:15 AM
- Database migration: Completed at 02:45 AM
- Pod restarts: Completed at 03:30 AM
- Total duration: 1 hour 15 minutes

---

## Post-Upgrade Validation

### Verify SMAX Version

Confirm all components are running version 24.4.

```bash
kubectl get pods -n {{namespace}} -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.containers[0].image}{"\n"}{end}'
```

- [ ] Step not completed

---

### Check All Pods Status

Ensure all pods are running.

```bash
kubectl get pods -n {{namespace}}
```

- [ ] Step not completed

---

### Disable Maintenance Mode

Disable maintenance mode to allow user access.

- [ ] Step not completed

---

## Functional Testing

### Test User Authentication

Verify login functionality.

- [ ] Step not completed

**Personal Notes:**
Tested authentication methods:
- ✓ Local users: Working
- ✓ LDAP authentication: Working
- ✓ SSO: Working

---

### Create Test Incident

Create a test incident to verify ITSM functionality.

- [ ] Step not completed

---

### Test Notifications

Verify email notifications are working.

- [ ] Step not completed

---

### Test Service Portal

Access the Service Portal and verify all widgets load.

- [ ] Step not completed

---

### Test Reporting

Run sample reports.

- [ ] Step not completed

---

## Post-Upgrade Optimization

### Review Resource Utilization

Monitor resource usage post-upgrade.

```bash
kubectl top pods -n {{namespace}}
```

- [ ] Step not completed

---

### Optimize Database

Run database maintenance tasks.

```bash
psql -h {{nfs_server}} -U postgres -d bo_db -c "VACUUM ANALYZE;"
```

- [ ] Step not completed

---

## Finalization

### Notify Stakeholders

Inform stakeholders of successful upgrade completion.

- [ ] Step not completed

**Personal Notes:**
Email sent to all stakeholders
- Upgrade completed successfully
- All systems operational
- No issues reported during testing
- Change ticket closed

---

### Update Documentation

Update internal documentation with new version info.

- [ ] Step not completed

---
