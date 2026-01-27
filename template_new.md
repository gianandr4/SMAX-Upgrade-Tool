
config:
  namespace: "itsma-client"
  ESM_NAMESPACE: "itsma-client"
  version: "24.6"
  registry: "registry.example.com"
  registry_server: "registry.example.com"
  nfs_server: "nfs.internal.com"
  username: "admin"
  password: "changeme"
  image_secret_name: "regcred"
  backup_location: "/mnt/backup"
  db_host: "postgres.internal.com"
  db_user: "postgres"
  db_name: "smax_db"


# CLIENT A - SMAX Upgrade 24.4 → 24.6

## Pre-Upgrade Planning

### Review Release Notes

Review the official SMAX release notes for version 24.6 to understand new features, changes, and potential breaking changes.

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

### Schedule Maintenance Window

Coordinate with stakeholders to schedule an appropriate maintenance window for the upgrade.

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

### Verify System Requirements

Ensure the infrastructure meets the minimum requirements for SMAX 24.6.

```bash
kubectl get nodes
kubectl describe nodes | grep -i resources
```

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

## Binaries Download

### Download SMAX 24.6 Binaries

Download the SMAX 24.6 installation binaries from the Micro Focus support portal.

```bash
wget https://downloads.microfocus.com/smax/24.6/smax-24.6.tar.gz
tar -xzf smax-24.6.tar.gz
cd smax-24.6
```

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

### Verify Binary Checksums

Verify the integrity of downloaded binaries using provided checksums.

```bash
sha256sum -c checksums.txt
```

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

### Upload Binaries to Registry

Upload the SMAX container images to your private registry.

```bash
./upload-images.sh -r <registry_server> -u <username> -p <password>
```

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

## Pre-Upgrade Backup

### Backup PostgreSQL Databases

Create full backups of all SMAX PostgreSQL databases (BoB, SmartAnalytics, IdM).

```bash
pg_dump -h <db_host> -U <db_user> <db_name> > <backup_location>/smax_backup_$(date +%Y%m%d_%H%M%S).sql
```

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

### Backup Kubernetes Configurations

Export all SMAX Kubernetes configurations for disaster recovery.

```bash
kubectl get all -n <namespace> -o yaml > <backup_location>/k8s-resources-backup-$(date +%Y%m%d).yaml
kubectl get pvc -n <namespace> -o yaml > <backup_location>/pvc-backup-$(date +%Y%m%d).yaml
kubectl get secrets -n <namespace> -o yaml > <backup_location>/secrets-backup-$(date +%Y%m%d).yaml
kubectl get configmaps -n <namespace> -o yaml > <backup_location>/configmaps-backup-$(date +%Y%m%d).yaml
```

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

### Backup Persistent Volume Data

Create snapshots or backups of all persistent volumes used by SMAX.

```bash
kubectl get pv
# Document PV locations and create appropriate backups
```

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

### Document Current Configuration

Document the current SMAX configuration including resource allocations, custom settings, and integrations.

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

## Pre-Upgrade Validation

### SMAX Health Check

Ensure all SMAX pods are in Running state with correct replica counts before starting the upgrade.

```bash
kubectl get pods -n <namespace>
kubectl get pods -n <namespace> | grep -v "Running\|Completed" | wc -l
```

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

### Verify Storage Availability

Ensure sufficient storage is available for the upgrade process.

```bash
kubectl get pvc -n <namespace>
df -h
```

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

### Check Resource Utilization

Review current CPU and memory utilization to ensure capacity for the upgrade.

```bash
kubectl top nodes
kubectl top pods -n <namespace>
```

#### Notes

#### Notes

**Personal Notes:**

### - [ ] Complete

### Verify Network Connectivity

Test network connectivity to external systems (LDAP, SMTP, databases, etc.).

```bash
kubectl run -it --rm debug --image=busybox --restart=Never -n <namespace> -- sh
# Inside the pod: ping, nslookup, telnet tests
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Run Pre-Upgrade Checker

Execute the SMAX pre-upgrade validation tool if available.

```bash
./pre-upgrade-check.sh -n <namespace>
```

### - [ ] Complete

#### Notes

**Personal Notes:**



## Registry Configuration

### Update Docker Registry Secret

Create or update the Docker registry secret for pulling SMAX 24.6 images.

```bash
kubectl delete secret <image_secret_name> -n <namespace> --ignore-not-found
kubectl create secret docker-registry <image_secret_name> \
  --docker-username=<username> \
  --docker-password=<password> \
  --docker-server=<registry_server> \
  -n <namespace>
kubectl get secret <image_secret_name> -n <namespace>
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Verify Registry Access

Test that Kubernetes can pull images from the registry.

```bash
kubectl run test-registry --image=<registry_server>/smax/sample:24.6 -n <namespace> --command -- sleep 30
kubectl get pods -n <namespace> | grep test-registry
kubectl delete pod test-registry -n <namespace>
```

### - [ ] Complete

#### Notes

**Personal Notes:**



## Upgrade Execution

### Place SMAX in Maintenance Mode

Enable maintenance mode to prevent users from accessing SMAX during the upgrade.

```bash
kubectl exec -it <suite-installer-pod> -n <namespace> -- /opt/sma/bin/maintenance-mode.sh enable
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Scale Down Non-Critical Services

Scale down non-critical services to minimize resource usage during upgrade.

```bash
kubectl scale deployment <deployment-name> --replicas=0 -n <namespace>
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Apply SMAX 24.6 Upgrade

Execute the main SMAX upgrade process using the suite installer.

```bash
./upgrade.sh -n <namespace> -v <version>
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Monitor Upgrade Progress

Monitor the upgrade process and watch for any errors or warnings.

```bash
kubectl get pods -n <namespace> -w
kubectl logs -f <suite-installer-pod> -n <namespace>
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Wait for Pod Restarts

Wait for all pods to restart and reach Running state after the upgrade.

```bash
kubectl get pods -n <namespace>
# Wait until all pods show Running status with correct ready count (e.g., 2/2)
```

### - [ ] Complete

#### Notes

**Personal Notes:**



## Post-Upgrade Validation

### Verify SMAX Version

Confirm that all SMAX components are running version 24.6.

```bash
kubectl get pods -n <namespace> -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.containers[0].image}{"\n"}{end}' | grep smax
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Database Schema Verification

Verify that database schema has been properly upgraded.

```bash
psql -h <db_host> -U <db_user> -d <db_name> -c "SELECT version FROM schema_version ORDER BY installed_rank DESC LIMIT 1;"
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Check All Pods Status

Ensure all pods are running with the correct number of replicas.

```bash
kubectl get pods -n <namespace>
kubectl get deployments -n <namespace>
kubectl get statefulsets -n <namespace>
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Verify Persistent Volume Claims

Ensure all PVCs are bound and functioning correctly.

```bash
kubectl get pvc -n <namespace>
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Check Service Endpoints

Verify that all Kubernetes services have proper endpoints.

```bash
kubectl get svc -n <namespace>
kubectl get endpoints -n <namespace>
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Review Pod Logs

Check pod logs for any errors or warnings after the upgrade.

```bash
kubectl logs <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace> --previous  # for crashed pods
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Disable Maintenance Mode

Disable maintenance mode to allow users to access SMAX.

```bash
kubectl exec -it <suite-installer-pod> -n <namespace> -- /opt/sma/bin/maintenance-mode.sh disable
```

### - [ ] Complete

#### Notes

**Personal Notes:**



## Functional Testing

### Test User Authentication

Verify that users can log in using all configured authentication methods (local, LDAP, SSO).

### - [ ] Complete

#### Notes

**Personal Notes:**



### Create Test Incident

Create a test incident to verify basic ITSM functionality.

### - [ ] Complete

#### Notes

**Personal Notes:**



### Test Notifications

Verify that email notifications are working correctly.

### - [ ] Complete

#### Notes

**Personal Notes:**



### Test Service Portal

Access the Service Portal and verify that it loads correctly with all widgets.

### - [ ] Complete

#### Notes

**Personal Notes:**



### Test Reporting

Run sample reports to ensure reporting functionality is working.

### - [ ] Complete

#### Notes

**Personal Notes:**



### Test Integrations

Verify that all external integrations (UCMDB, Operations Bridge, etc.) are functioning.

### - [ ] Complete

#### Notes

**Personal Notes:**



### Test Mobile Access

If applicable, test SMAX mobile application connectivity and functionality.

### - [ ] Complete

#### Notes

**Personal Notes:**



### Performance Testing

Conduct basic performance tests to ensure response times are acceptable.

### - [ ] Complete

#### Notes

**Personal Notes:**



## Post-Upgrade Optimization

### Review Resource Utilization

Monitor resource usage and adjust resource limits if necessary.

```bash
kubectl top pods -n <namespace>
kubectl describe pod <pod-name> -n <namespace> | grep -A 5 Resources
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Optimize Database Performance

Run database maintenance tasks (vacuum, analyze, reindex) if needed.

```bash
psql -h <db_host> -U <db_user> -d <db_name> -c "VACUUM ANALYZE;"
```

### - [ ] Complete

#### Notes

**Personal Notes:**



### Review and Update Documentation

Update internal documentation to reflect the new SMAX version and any configuration changes.

### - [ ] Complete

#### Notes

**Personal Notes:**



### Clear Old Container Images

Remove old SMAX container images from nodes to free up disk space.

```bash
kubectl get nodes
# SSH to each node and run: docker image prune -a --filter "until=24h"
```

### - [ ] Complete

#### Notes

**Personal Notes:**



## Finalization

### Notify Stakeholders

Inform all stakeholders that the upgrade has been completed successfully.

### - [ ] Complete

#### Notes

**Personal Notes:**



### Update Change Records

Close the change ticket and document the upgrade process and any issues encountered.

### - [ ] Complete

#### Notes

**Personal Notes:**



### Schedule Follow-up Review

Schedule a follow-up meeting to review upgrade success and address any outstanding issues.

### - [ ] Complete

#### Notes

**Personal Notes:**



### Archive Upgrade Documentation

Archive all upgrade-related documentation, logs, and scripts for future reference.

### - [ ] Complete

#### Notes

**Personal Notes:**


