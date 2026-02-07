---
config:
  namespace: itsma-clienta
  ESM_NAMESPACE: itsma-clienta
  version: '24.6'
  registry: registry.example.com
  registry_server: registry.example.com
  nfs_server: nfs.internal.com
  username: admin
  password: changeme
  image_secret_name: regcred
---

# Client A Upgrade to v24.6

## Preparation

### SMAX Health Check

Ensure all pods are in 2/2 or 1/1 Running state before starting.

```bash
kubectl get pods -n {{namespace}}
```

#### Notes

##### 24.4

Current version notes hereasda

##### 24.5

dadaasda

### - [ ] Complete

### Create Docker Registry Secret

Create secret for pulling images from private registry.

```bash
kubectl get secret <image_secret_name> -n <ESM_NAMESPACE>
```

#### Notes

##### 24.4

Some notes about the registry secret creation.

### - [ ] Complete

### asda

asda









## Upgrade

### Monitor Upgrade Progress

Watch pods restart and come back online.

```bash
kubectl get pods -n <ESM_NAMESPACE> -w
```

#### Notes

##### 24.3

In previous versions, this step took longer.

##### 24.4

Monitor the upgrade process carefully.

### - [ ] Complete

### Verify SMAX Version

Confirm the upgrade was successful.

```bash
kubectl get pods -n {{namespace}} -o jsonpath='{.items[0].spec.containers[0].image}'
```

#### Notes

##### 24.4

Check that the version matches the expected 24.4 version.

### - [ ] Complete

## Verification

### Run Smoke Tests

Test basic functionality: login, create ticket, check reports.

#### Notes

##### 24.4

Run the standard smoke test suite.

### - [ ] Complete

### - [ ] Complete</content>

<parameter name="filePath">c:\Users\Andreas\Desktop\Projects\SMAX-Upgrade-Tool\SMAX\24.4\client-D-sample.md

### - [ ] Complete

