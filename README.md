![David Skowronski logo](https://avatars.githubusercontent.com/u/11982214?s=96&v=4)
# Rampartai Security Helm Charts

[![License][license-img]][license]

[license-img]: https://img.shields.io/badge/license-MIT-blue
[license]: https://github.com/dskow/helm-charts/blob/main/LICENSE

Open source helm charts on a [Kubernetes](https://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Charts

- [rampart-tetragon](https://github.com/dskow/rampart-tetragon/tree/main/deploy/helm)

## Prerequisites

- kubernetes cluster

#### Installing [Helm](https://helm.sh)

```
curl -L https://git.io/get_helm.sh | bash
helm init
```
# Install Tetragon

This needs to be installed first so it sets up the networking correctly.

```
helm repo add cilium https://helm.cilium.io
helm repo update
helm install tetragon cilium/tetragon -n kube-system
kubectl rollout status -n kube-system ds/tetragon -w
```

Detailed guide for tetragon installation is at [Install Tetragon](https://tetragon.io/docs/getting-started/install-k8s/)

The dnscore subnets should not change to 10.0.0.x subnet. Use this command to see the subnets for the kubernetes pods.
```
kubectl get pod --all-namespaces -o wide
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
openssl base64 -in ./kubetest-quake3stack-agent-cert.pfx -out pem.pem
```

Take all the lines pem file, reduce to one line, and paste into cert field of my-cred.yaml

Here is command to reduce pem file to one line.

```
tr -d "\n" < pem.pem
```

In notepad++, type the passphrase, select it and choose Plugins->MIMI Tools->Base64 Encode and paste that into key field of my-cred.yaml

Here is an example command to generate the encoded key.

```
echo -n 'my_cert_key' | base64
```

Install rampart-tetragon helm chart into the rampart-system namespace.

```
helm install -f my-cert.yaml rampart-tetragon rampartai/rampart-tetragon -n rampart-system --create-namespace
helm status rampart-tetragon -n rampart-system
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
helm uninstall rampart-tetragon -n rampart-system
```
