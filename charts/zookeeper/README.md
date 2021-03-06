# Zookeeper Helm Chart

Installs Zookeeper clusters atop Kubernetes.

## Introduction

This chart creates a Zookeeper cluster in [Kubernetes](http://kubernetes.io) using the [Helm](https://helm.sh) package manager. The chart can be installed multiple times to create Zookeeper cluster in multiple namespaces.

## Prerequisites

  - Kubernetes 1.15+ with Beta APIs
  - Helm 3.2.1+
  - Zookeeper Operator. You can install it using its own [Helm chart](https://github.com/pravega/zookeeper-operator/tree/master/charts/zookeeper-operator)

## Installing the Chart

To install the zookeeper chart, use the following commands:

```
$ helm repo add pravega https://charts.pravega.io
$ helm repo update
$ helm install [RELEASE_NAME] pravega/zookeeper --version=[VERSION]
```
where:
- **[RELEASE_NAME]** is the release name for the zookeeper chart.
- **[CLUSTER_NAME]** is the name of the zookeeper cluster so created. (If [RELEASE_NAME] contains the string `zookeeper`, `[CLUSTER_NAME] = [RELEASE_NAME]`, else `[CLUSTER_NAME] = [RELEASE_NAME]-zookeeper`. The [CLUSTER_NAME] can however be overridden by providing `--set fullnameOverride=[CLUSTER_NAME]` along with the helm install command)
- **[VERSION]** can be any stable release version for zookeeper from 0.2.8 onwards.

This command deploys zookeeper on the Kubernetes cluster in its default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

## Upgrading the Chart

To upgrade the zookeeper chart from version **[OLD_VERSION]** to version **[NEW_VERSION]**, use the following command:

```
$ helm upgrade [RELEASE_NAME] pravega/zookeeper --version=[NEW_VERSION] --set image.tag=[NEW_VERSION] --reuse-values --timeout 600s
```
**Note:** By specifying the `--reuse-values` option, the configuration of all parameters are retained across upgrades. However if some values need to be modified during the upgrade, the `--set` flag can be used to specify the new configuration for these parameters. Also, by skipping the `reuse-values` flag, the values of all parameters are reset to the default configuration that has been specified in the published charts for version [NEW_VERSION].

## Uninstalling the Chart

To uninstall/delete the zookeeper chart, use the following command:

```
$ helm uninstall [RELEASE_NAME]
```

This command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the zookeeper chart and their default values.

| Parameter | Description | Default |
| ----- | ----------- | ------ |
| `replicas` | Expected size of the zookeeper cluster (valid range is from 1 to 7) | `3` |
| `image.repository` | Image repository | `pravega/zookeeper` |
| `image.tag` | Image tag | `0.2.8` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `domainName` | External host name appended for dns annotation | |
| `kubernetesClusterDomain` | Domain of the kubernetes cluster | `cluster.local` |
| `labels` | Specifies the labels to be attached | `{}` |
| `ports` | Groups the ports for a zookeeper cluster node for easy access | `[]` |
| `pod` | Defines the policy to create new pods for the zookeeper cluster | `{}` |
| `pod.labels` | Labels to attach to the pods | `{}` |
| `pod.nodeSelector` | Map of key-value pairs to be present as labels in the node in which the pod should run | `{}` |
| `pod.affinity` | Specifies scheduling constraints on pods | `{}` |
| `pod.resources` | Specifies resource requirements for the container | `{}` |
| `pod.tolerations` | Specifies the pod's tolerations | `[]` |
| `pod.env` | List of environment variables to set in the container | `[]` |
| `pod.annotations` | Specifies the annotations to attach to pods | `{}` |
| `pod.securityContext` | Specifies the security context for the entire pod | `{}` |
| `pod.terminationGracePeriodSeconds` | Amount of time given to the pod to shutdown normally | `30` |
| `config.initLimit` | Amount of time (in ticks) to allow followers to connect and sync to a leader | `10` |
| `config.tickTime` | Length of a single tick which is the basic time unit used by Zookeeper (measured in milliseconds) | `2000` |
| `config.syncLimit` | Amount of time (in ticks) to allow followers to sync with Zookeeper | `2` |
| `config.quorumListenOnAllIPs` | Whether Zookeeper server will listen for connections from its peers on all available IP addresses | `false` |
| `storageType` | Type of storage that can be used it can take either ephemeral or persistence as value | `persistence` |
| `persistence.reclaimPolicy` | Reclaim policy for persistent volumes | `Delete` |
| `persistence.storageClassName` | Storage class for persistent volumes | `standard` |
| `persistence.volumeSize` | Size of the volume requested for persistent volumes | `20Gi` |
| `ephemeral.emptydirvolumesource.medium` |  What type of storage medium should back the directory. | `""` |
| `ephemeral.emptydirvolumesource.sizeLimit` | Total amount of local storage required for the EmptyDir volume. | `20Gi` |
| `containers` | Application containers run with the zookeeper pod | `[]` |
| `volumes` | Named volumes that may be accessed by any container in the pod | `[]` |
