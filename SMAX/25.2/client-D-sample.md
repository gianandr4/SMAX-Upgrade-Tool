---
config:
  namespace: itsma-clienta
  ESM_NAMESPACE: itsma-clienta
  version: '25.2'
  registry: registry.example.com
  registry_server: registry.example.com
  nfs_server: nfs.internal.com
  username: admin
  password: changeme
  image_secret_name: regcred
  asd: da
---

# Client A Upgrade to v25.2

## Preparation

### SMAX Health Check

Ensure all pods are in 2/2 or 1/1 Running state before starting.

```bash
kubectl get pods -n {{namespace}}
```

#### Notes

![image.png](https://raw.githubusercontent.com/gianandr4/SMAX-Upgrade-Tool/main/SMAX/24.4/images/1769514572183-image.png)
Current version notes 

##### 24.4

Current version notes here
asda

### - [x] Complete

### Create Docker Registry Secret

Create secret for pulling images from private registry.

```bash
kubectl get secret <image_secret_name> -n <ESM_NAMESPACE>
```

#### Notes

Some notes about the registry secret creation.

### - [ ] Complete

## Upgrade

### Monitor Upgrade Progress

Watch pods restart and come back online.

```bash
kubectl get pods -n <ESM_NAMESPACE> -w
```

#### Notes

Monitor the upgrade process carefully.

##### 24.3

In previous versions, this step took longer.

### - [x] Complete

### Verify SMAX Version

Confirm the upgrade was successful.

```bash
kubectl get pods -n {{namespace}} -o jsonpath='{.items[0].spec.containers[0].image}'
```

#### Notes

Check that the version matches the expected 24.4 version.

### - [ ] Complete

## Verification

### Run Smoke Tests

Test basic functionality: login, create ticket, check reports.

#### Notes

Run the standard smoke test suite.

##### 24.4

New features in 24.4 require additional testing.

### - [ ] Complete

### - [ ] Complete</content>

<parameter name="filePath">c:\Users\Andreas\Desktop\Projects\SMAX-Upgrade-Tool\SMAX\24.4\client-D-sample.md

### - [ ] Complete

