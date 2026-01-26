---
config:
  namespace: itsma-cliente
  ESM_NAMESPACE: itsma-cliente
  version: '24.4'
  registry_server: harbor.example.com
  username: sysadmin
  password: SecurePass123
  image_secret_name: harbor-secret
---

# Pre-Flight Checks

## Review Release Notes

Read the 24.4 release notes and understand new features and breaking changes.

- [x] Step completed

**Personal Notes:**
# Key Points from Release Notes

✅ **New Features:**
- Enhanced API performance (30% faster)
- New dashboard widgets for analytics
- Improved mobile responsiveness

⚠️ **Breaking Changes:**
- Legacy API endpoints deprecated
- New authentication flow required

📋 **Action Items:**
- Update API client libraries
- Test mobile app compatibility

---

## Check System Resources

Verify adequate CPU, memory, and storage for upgrade.

```bash
kubectl top nodes
df -h
```

- [x] Step completed

---

## Backup Current Configuration

Export all ConfigMaps and Secrets.

```bash
kubectl get configmaps -n {{namespace}} -o yaml > backup-configmaps.yaml
kubectl get secrets -n {{namespace}} -o yaml > backup-secrets.yaml
```

- [ ] Step not completed

---

# Environment Preparation

## Create Registry Secret

Setup authentication for private Docker registry.

```bash
kubectl create secret docker-registry <image_secret_name> \
  --docker-username=<username> \
  --docker-****** \
  --docker-server=<registry_server> \
  -n <ESM_NAMESPACE>
```

- [ ] Step not completed

---

## Download Upgrade Package

Pull the upgrade images to local registry.

```bash
docker pull <registry_server>/smax/suite:{{version}}
docker pull <registry_server>/smax/idm:{{version}}
docker pull <registry_server>/smax/autopass:{{version}}
```

- [ ] Step not completed

---

# Upgrade Execution

## Enable Maintenance Mode

Put SMAX in maintenance mode to prevent user access during upgrade.

- [ ] Step not completed

**Personal Notes:**
**Maintenance Window:** 
- Start: Saturday, January 20, 2024 - 2:00 AM EST
- Duration: 3-4 hours
- End: Saturday, January 20, 2024 - 6:00 AM EST

**Communication:**
- Notification sent to all users via email
- Status page updated
- On-call team notified

---

## Execute Upgrade Script

Run the automated upgrade process.

```bash
./upgrade.sh -n {{namespace}} -v {{version}} -r <registry_server>
```

- [ ] Step not completed

---

## Monitor Pod Status

Watch all pods restart and achieve ready state.

```bash
kubectl get pods -n <ESM_NAMESPACE> -w
kubectl wait --for=condition=Ready pods --all -n {{namespace}} --timeout=30m
```

- [ ] Step not completed

---

# Post-Upgrade Validation

## Verify Version

Check that all components are running the new version.

```bash
kubectl get deployments -n {{namespace}} -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.template.spec.containers[0].image}{"\n"}{end}'
```

- [ ] Step not completed

---

## Health Check

Verify all services are healthy and responding.

```bash
kubectl get pods -n <ESM_NAMESPACE>
curl -k https://smax.example.com/health
```

- [ ] Step not completed

---

## Functional Testing

Test core application functionality.

- [ ] Step not completed

**Personal Notes:**
### Smoke Test Checklist

**Authentication:**
- [ ] Login with admin user
- [ ] Login with regular user
- [ ] SSO integration working

**Core Functions:**
- [ ] Create incident ticket
- [ ] Assign ticket to agent
- [ ] Add comments/attachments
- [ ] Close ticket

**Integrations:**
- [ ] Email notifications working
- [ ] LDAP sync functional
- [ ] API endpoints responding

**Reports:**
- [ ] Dashboard loads correctly
- [ ] Standard reports generate
- [ ] Custom reports functional

---

## Disable Maintenance Mode

Return SMAX to normal operation.

- [ ] Step not completed

---
