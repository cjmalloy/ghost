# Ghost

[Ghost](https://ghost.org/) is one of the most versatile open source content management systems on the market.

This chart uses the official Docker image for Ghost. It provides much better stability and customization than the bitnami docker image and chart.

## TL;DR;

```console
$ unzip ghost.zip
$ helm install ghost/
```

## Introduction

This chart bootstraps a [Ghost](https://github.com/docker-library/ghost) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.12+
- Helm 3
- PV provisioner support in the underlying infrastructure
- ReadWriteMany volumes for deployment scaling

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ unzip ghost.zip
$ helm install --name my-release ghost/
```

The command deploys Ghost on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```console
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

The following table lists the configurable parameters of the Ghost chart and their default values.

| Parameter                           | Description                                                   | Default                                                  |
|-------------------------------------|---------------------------------------------------------------|----------------------------------------------------------|
| `image.repository`                  | Ghost Image name                                              | `ghost`                                          |
| `image.pullPolicy`                  | Image pull policy                                             | `IfNotPresent`                                           |
| `config`                            | Add environment variables to the container                    | `{}` (use default docker config)                         |
| `configSecrets`                     | Add environment variables to the container using secrets      | `{}` (use default docker config)                         |
| `imagePullSecrets`                  | Specify docker-registry secret names as an array              | `[]` (does not add image pull secrets to deployed pods)  |
| `nameOverride`                      | String to partially override ghost.fullname template with a string (will prepend the release name) | `nil`               |
| `fullnameOverride`                  | String to fully override ghost.fullname template with a string                                     | `nil`               |
| `livenessProbe.enabled`             | Would you like a livenessProbe to be enabled                  | `true`                                                   |
| `livenessProbe.initialDelaySeconds` | Delay before liveness probe is initiated                      | 120                                                      |
| `livenessProbe.periodSeconds`       | How often to perform the probe                                | 3                                                        |
| `livenessProbe.timeoutSeconds`      | When the probe times out                                      | 5                                                        |
| `livenessProbe.failureThreshold`    | Minimum consecutive failures to be considered failed          | 6                                                        |
| `livenessProbe.successThreshold`    | Minimum consecutive successes to be considered successful     | 1                                                        |
| `readinessProbe.enabled`            | Would you like a readinessProbe to be enabled                 | `true`                                                   |
| `readinessProbe.initialDelaySeconds`| Delay before readiness probe is initiated                     | 30                                                       |
| `readinessProbe.periodSeconds`      | How often to perform the probe                                | 3                                                        |
| `readinessProbe.timeoutSeconds`     | When the probe times out                                      | 5                                                        |
| `readinessProbe.failureThreshold`   | Minimum consecutive failures to be considered failed          | 6                                                        |
| `readinessProbe.successThreshold`   | Minimum consecutive successes to be considered successful     | 1                                                        |
| `securityContext.enabled`           | Enable security context                                       | `true`                                                   |
| `securityContext.fsGroup`           | Group ID for the container                                    | `1001`                                                   |
| `securityContext.runAsUser`         | User ID for the container                                     | `1001`                                                   |
| `service.type`                      | Kubernetes Service type                                       | `ClusterIP`                                           |
| `service.port`                      | Service HTTP port                                             | `80`                                                     |
| `service.nodePort`                  | Kubernetes http node port                                     | `nil`                                                     |
| `service.externalTrafficPolicy`     | Enable client source IP preservation                          | `Cluster`                                                |
| `service.loadBalancerIP`            | LoadBalancerIP for the Ghost service                          | ``                                                       |
| `service.annotations`               | Service annotations                                           | ``                                                       |
| `ingress.enabled`                   | Enable ingress controller resource                            | `false`                                                  |
| `ingress.annotations`               | Ingress annotations                                           | `[]`                                                     |
| `persistence.enabled`               | Enable persistence using PVC                                  | `true`                                                   |
| `persistence.storageClass`          | PVC Storage Class for Ghost volume                            | `nil` (uses alpha storage annotation)                    |
| `persistence.accessMode`            | PVC Access Mode for Ghost volume                              | `ReadWriteOnce`                                          |
| `persistence.size`                  | PVC Storage Request for Ghost volume                          | `8Gi`                                                    |
| `persistence.path`                  | Path to mount the volume at, to use other images              | `/bitnami`                                               |
| `resources`                         | CPU/Memory resource requests/limits                           | Memory: `512Mi`, CPU: `300m`                             |
| `nodeSelector`                      | Node selector for pod assignment                              | `{}`                                                     |
| `affinity`                          | Map of node/pod affinities                                    | `{}`                                                     |

> **Note**:
>
> For the Ghost application function correctly, you should specify the `ghostHost` parameter to specify the FQDN (recommended) or the public IP address of the Ghost service.
>
> Optionally, you can specify the `ghostLoadBalancerIP` parameter to assign a reserved IP address to the Ghost service of the chart. However please note that this feature is only available on a few cloud providers (f.e. GKE).
>
> To reserve a public IP address on GKE:
>
> ```bash
> $ gcloud compute addresses create ghost-public-ip
> ```
>
> The reserved IP address can be assigned to the Ghost service by specifying it as the value of the `ghostLoadBalancerIP` parameter while installing the chart.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```console
$ helm install --name my-release \
  --set ghost.configSecrets.database__connection__password=dbPass \
    ghost/
```

Alternatively, a YAML file that specifies the values for the above parameters can be provided while installing the chart. For example,

```console
$ helm install --name my-release -f values.yaml ghost/
```

> **Tip**: You can use the default [values.yaml](values.yaml)
