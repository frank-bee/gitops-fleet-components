This is a set of infrastructure components (Kustomizations) encapsulated in a top-level component 
which can be used to bootstrap a cluster / environmennt.

## How to use
Reference them in your environment cluster as follows.
Modify `<tag>` to match the tag of the version you want to use.

```
---
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: infrastructure
  namespace: flux-system
spec:
  interval: 1m
  url: https://github.com/frank-bee/gitops-fleet-components
  ref:
    tag: <tag>
---
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
kind: Kustomization
metadata:
  name: infrastructure
  namespace: flux-system
spec:
  interval: 1h
  retryInterval: 1m
  timeout: 5m
  prune: true
  wait: true
  sourceRef:
    kind: GitRepository
    name: infrastructure
  path: ./infrastructure
  patches:
    - patch: |-
        spec:
          sourceRef:
            tag: <tag>      
      target:
        kind: Kustomization
```