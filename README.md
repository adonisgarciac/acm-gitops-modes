# acm-gitops-modes
## Prerequisites

- Two or more OCP clusters installed.
- Red Hat ACM working in one cluster (operator and MultiClusterHub installed).
- Red Hat OpenShift GitOps installed in every cluster with an ArgoCD instance.
- Rest of clusters attached to RH ACM as managed clusters. One of them must have cluster1 as name.
- Rest of clusters are part of the managedcluster ClusterSet in RH ACM. We can create managedcluster ClusterSet using acm/acm_managedclusterset.yaml. Launch following command on ACM cluster:
```bash
    oc apply -f acm/acm_managedclusterset.yaml
```

## Create ACM needed resources

### Global
Global objects (placement, gitopscluster and managedclustersetbinding) are for push mode. It will include configuration for hub cluster as an additional managed cluster.

1. Placement

```bash
    oc apply -f acm/acm_global_placement.yaml
```

2. ManagedClusterSetBinding to bind openshift-gitops with global placement

```bash
    oc apply -f acm/acm_global_managedclustersetginding.yaml
```

3. ACM GitOps cluster for global placement

```bash
    oc apply -f acm/acm_global_gitopscluster.yaml
```

### ManagedCluster
ManagedCluster objects (placement, gitopscluster and managedclustersetbinding) are for pull mode. It is not possible to use pull mode for global placement if hub cluster is part of managed clusters.


1. Placement

```bash
    oc apply -f acm/acm_managedcluster_placement.yaml
```

2. ManagedClusterSetBinding to bind openshift-gitops with global placement

```bash
    oc apply -f acm/acm_managedcluster_managedclustersetginding.yaml
```

3. ACM GitOps cluster for global placement

```bash
    oc apply -f acm/acm_managedcluster_gitopscluster.yaml
```

## Create ArgoCD needed resources
### On every managed cluster (except HUB cluster)

1. ACM project in ArgoCD

```bash
    oc apply -f argocd/argocd_acm_project.yaml
```

2. hello-world namespace
```bash
    oc apply -f argocd/argocd/argocd_helloworld_ns.yaml
```

### On HUB cluster

1. ACM project in ArgoCD

```bash
    oc apply -f argocd/argocd_acm_project.yaml
```

2. Init ArgoCD app which will create appsets

```bash
    oc apply -f argocd_init_app.yaml
```