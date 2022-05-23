# Multi-Cluster Metallb IP Pool Example

This example can be used to used to deploy metallb in multiple cluster with individual IP pool allocated for each cluster 
using kustomize. 
Below example has clustergroup "metallb-ip-pool" which includes downstream clusters.


```yaml
apiVersion: fleet.cattle.io/v1alpha1
kind: GitRepo
metadata:
  name: metallb-multisite
  namespace: fleet-default
spec:
  branch: main
  paths:
  - kustomize
  repo: https://github.com/DCSyd/metallb
  targets:
  - clusterGroup: metallb-ip-pool
```


* ippool folder has sub-folder for each site, which will have site specific IP pool in metallb.yaml.
* ippool folder also contains fleet.yaml which matches the specified label for each cluster and then maps to related folder for that cluster under ippool folder.

```yaml
defaultNamespace: metallb-system
targetCustomizations:
- name: site1
  clusterSelector:
    matchLabels:
      site: site1
  kustomize:
    dir: ippool/site1
- name: site2
  clusterSelector:
    matchLabels:
      site: site2
  kustomize:
    dir: ippool/site2
```
