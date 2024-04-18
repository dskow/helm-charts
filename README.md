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

Generate yaml file to hold the cert and passcode for installation.

```
helm show values rampartai/rampart-tetragon > my-cert.yaml
```

# Update cert 
Update my-cert.yaml with the correct values using your favorite editor.

convert agent pfx to crt

**NOTE**: obtain my-agent-cert.pfx via an Rampart-ai representative.

```
openssl pkcs12 -in my-agent-cert.pfx -clcerts -nokeys -out temp.crt -legacy
```

Take all the lines between BEGIN and END in crt file and past into cert field of my-cred.yaml

```
-----BEGIN CERTIFICATE-----
MII
...
E4M=
-----END CERTIFICATE-----
```

In notepad++, type the passphrase, select it and choose Plugins->MIMI Tools->Base64 Encode and paste that into key field of my-cred.yaml

Install specific helm chart
```
helm install -f my-cert.yaml rampart-tetragon rampartai/rampart-tetragon -n kube-system
helm status rampart-tetragon -n kube-system
```

See status of pods

```
kubectl get pods --all-namespaces -o wide
```

See logs for rampart agent

```
kubectl logs -n kube-system daemonset/rampart-agent-daemonset -f
```

Uninstall helm chart

```
helm uninstall rampart-tetragon -n kube-system
```
