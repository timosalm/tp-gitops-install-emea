# Tanzu Platform for K8s EMEA GitOps Install

## Prerequisites

- Install the Carvel tools [kapp](https://carvel.dev/kapp/docs/v0.63.x/install/) and [ytt](https://carvel.dev/ytt/docs/v0.50.x/install/). If you choose the script install, all of them will be installed together. 

### Install kapp-controller
```
kapp deploy -a kc -f https://github.com/carvel-dev/kapp-controller/releases/latest/download/release.yml
```
### Install secretgen-controller
```
kapp deploy -a sg -f https://github.com/carvel-dev/secretgen-controller/releases/latest/download/release.yml
```

## Deploy "emea-tp-main" cluster sync
```
(cd clusters/emea-tp-main && ytt -f sync/config --data-values-file sync/values.yaml | kapp deploy -a sync -f -)
```

