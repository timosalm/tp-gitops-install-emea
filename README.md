# Tanzu Platform for K8s EMEA GitOps Install

## Prerequisites

- Install the Carvel tools [kapp](https://carvel.dev/kapp/docs/v0.63.x/install/) and [ytt](https://carvel.dev/ytt/docs/v0.50.x/install/). If you choose the script install, all of them will be installed together.

## Main cluster

### Prerequisites

#### Install kapp-controller
Only necessary if cluster is/will not be attached to TMC / Tanzu Platform Kubernetes Operartions.

```
kapp deploy -a kc -f https://github.com/carvel-dev/kapp-controller/releases/latest/download/release.yml
```
#### Install secretgen-controller
Only necessary if cluster is/will not be attached to TMC / Tanzu Platform Kubernetes Operartions.

```
kapp deploy -a sg -f https://github.com/carvel-dev/secretgen-controller/releases/latest/download/release.yml
```

### Deploy "emea-tp-main" cluster sync
```
(cd clusters/emea-tp-main && ytt -f sync/config --data-values-file sync/values.yaml | kapp deploy -a sync -y -f -)
```

### Deploy "emea-east-aws-1" cluster sync
```
(cd clusters/emea-east-aws-1 && ytt -f sync/config --data-values-file sync/values.yaml | kapp deploy -a sync -y -f -)
```

## Workload clusters

### Add TP for K8s Capabilities
```
tanzu operations clustergroup use <sa-region>-workload
ytt -f tp4k8s/cluster-group | kubectl apply -f -
```
