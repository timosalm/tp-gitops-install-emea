# Tanzu Platform for K8s EMEA GitOps Install

## Prerequisites

- Install the Carvel tools [kapp](https://carvel.dev/kapp/docs/v0.63.x/install/) and [ytt](https://carvel.dev/ytt/docs/v0.50.x/install/). If you choose the script install, all of them will be installed together.

## Main cluster

### Prerequisites

#### Install kapp-controller
```
kapp deploy -a kc -f https://github.com/carvel-dev/kapp-controller/releases/latest/download/release.yml
```
#### Install secretgen-controller
```
kapp deploy -a sg -f https://github.com/carvel-dev/secretgen-controller/releases/latest/download/release.yml
```

### Deploy "emea-tp-main" cluster sync
```
(cd clusters/emea-tp-main && ytt -f sync/config --data-values-file sync/values.yaml | kapp deploy -a sync -y -f -)
```

## Virtual Workload clusters

### Get kubeconfig for workload clusters
```
vcluster connect tp-vcluster-1 -n tp-vcluster-1 --print --server=https://tp-vcluster-1.main.emea.end2end.link > ~/Downloads/tp-vcluster-1-kubeconfig.yaml
```

## Add TP for K8s Capabilities

### Add overlays
```
kubectl apply -f tp4k8s/capabilities/overlays
kubectl annotate pkgi tcs.tanzu.vmware.com -n tanzu-cluster-group-system ext.packaging.carvel.dev/ytt-paths-from-secret-name.0=tcs-overlay-secret
kubectl annotate pkgi egress.tanzu.vmware.com -n tanzu-cluster-group-system ext.packaging.carvel.dev/ytt-paths-from-secret-name.0=egress-overlay-secret
```
