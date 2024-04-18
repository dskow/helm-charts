![David Skowronski logo](https://avatars.githubusercontent.com/u/11982214?s=96&v=4)
# Rampartai Security Helm Charts

[![License][license-img]][license]

[license-img]: https://img.shields.io/badge/license-MIT-blue
[license]: https://github.com/dskow/helm-charts/blob/main/LICENSE

Open source projects helm chars on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Charts

- [rampart-tetragon](https://github.com/dskow/rampart-tetragon/tree/main/deploy/helm)

## Prerequisites

- kubernetes cluster

#### Installing [Helm](https://helm.sh)

```
curl -L https://git.io/get_helm.sh | bash
helm init
```


## Installing Dskow Community Helm Repository

```
helm repo add rampartai https://dskow.github.io/helm-charts/
helm repo update
```

## Installing the Chart

Search all the repositories available
```
helm search repo rampartai -l
```

Install specific helm chart
```
helm install rampart-tetragon rampartai/rampart-tetragon
helm status rampart-tetragon
