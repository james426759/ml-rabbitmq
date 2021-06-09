# RabbitMQ

[RabbitMQ](https://www.rabbitmq.com/) is an open source message broker software that implements the Advanced Message Queuing Protocol (AMQP).

## TL;DR

```bash
$ helm repo add bitnami https://charts.bitnami.com/bitnami
$ helm install my-release bitnami/rabbitmq
```

## Introduction

This chart bootstraps a [RabbitMQ](https://github.com/bitnami/bitnami-docker-rabbitmq) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

Bitnami charts can be used with [Kubeapps](https://kubeapps.com/) for deployment and management of Helm Charts in clusters. This chart has been tested to work with NGINX Ingress, cert-manager, fluentd and Prometheus on top of the [BKPR](https://kubeprod.io/).

## Prerequisites

- Kubernetes 1.12+
- Helm 3.1.0
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install my-release bitnami/rabbitmq
```

The command deploys RabbitMQ on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

The following table lists the configurable parameters of the RabbitMQ chart and their default values.

| Parameter                 | Description                                     | Default                                                 |
|---------------------------|-------------------------------------------------|---------------------------------------------------------|
| `global.imageRegistry`    | Global Docker image registry                    | `nil`                                                   |
| `global.imagePullSecrets` | Global Docker registry secret names as an array | `[]` (does not add image pull secrets to deployed pods) |
| `global.storageClass`     | Global storage class for dynamic provisioning   | `nil`                                                   |

### Common parameters

| Parameter          | Description                                                          | Default         |
|--------------------|----------------------------------------------------------------------|-----------------|
| `nameOverride`     | String to partially override rabbitmq.fullname                       | `nil`           |
| `fullnameOverride` | String to fully override rabbitmq.fullname                           | `nil`           |
| `clusterDomain`    | Default Kubernetes cluster domain                                    | `cluster.local` |
| `kubeVersion`      | Force target Kubernetes version (using Helm capabilities if not set) | `nil`           |

### RabbitMQ parameters

| Parameter                          | Description                                                                                                                              | Default                                                 |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------|
| `image.registry`                   | RabbitMQ image registry                                                                                                                  | `docker.io`                                             |
| `image.repository`                 | RabbitMQ image name                                                                                                                      | `bitnami/rabbitmq`                                      |
| `image.tag`                        | RabbitMQ image tag                                                                                                                       | `{TAG_NAME}`                                            |
| `image.pullPolicy`                 | RabbitMQ image pull policy                                                                                                               | `IfNotPresent`                                          |
| `image.pullSecrets`                | Specify docker-registry secret names as an array                                                                                         | `[]` (does not add image pull secrets to deployed pods) |
| `image.debug`                      | Set to true if you would like to see extra information on logs                                                                           | `false`                                                 |
| `auth.username`                    | RabbitMQ application username                                                                                                            | `user`                                                  |
| `auth.password`                    | RabbitMQ application password                                                                                                            | _random 10 character long alphanumeric string_          |
| `auth.existingPasswordSecret`      | Existing secret with RabbitMQ credentials (must contain a value for `rabbitmq-password` key)                                             | `nil` (evaluated as a template)                         |
| `auth.erlangCookie`                | Erlang cookie                                                                                                                            | _random 32 character long alphanumeric string_          |
| `auth.existingErlangSecret`        | Existing secret with RabbitMQ Erlang cookie (must contain a value for `rabbitmq-erlang-cookie` key)                                      | `nil` (evaluated as a template)                         |
| `auth.tls.enabled`                 | Enable TLS support on RabbitMQ                                                                                                           | `false`                                                 |
| `auth.tls.failIfNoPeerCert`        | When set to true, TLS connection will be rejected if client fails to provide a certificate                                               | `true`                                                  |
| `auth.tls.sslOptionsVerify`        | Should [peer verification](https://www.rabbitmq.com/ssl.html#peer-verification) be enabled?                                              | `verify_peer`                                           |
| `auth.tls.caCertificate`           | Certificate Authority (CA) bundle content                                                                                                | `nil`                                                   |
| `auth.tls.serverCertificate`       | Server certificate content                                                                                                               | `nil`                                                   |
| `auth.tls.serverKey`               | Server private key content                                                                                                               | `nil`                                                   |
| `auth.tls.existingSecret`          | Existing secret with certificate content to RabbitMQ credentials                                                                         | `nil`                                                   |
| `auth.tls.existingSecretFullChain` | Whether or not the existing secret contains the full chain in the certificate (`tls.crt`). Will be used in place of `ca.cert` if `true`. | `false`                                                 |
| `logs`                             | Path of the RabbitMQ server's Erlang log file                                                                                            | `-`                                                     |
| `ulimitNofiles`                    | Max File Descriptor limit                                                                                                                | `65536`                                                 |
| `maxAvailableSchedulers`           | RabbitMQ maximum available scheduler threads                                                                                             | `2`                                                     |
| `onlineSchedulers`                 | RabbitMQ online scheduler threads                                                                                                        | `1`                                                     |
| `memoryHighWatermark.enabled`      | Enable configuring Memory high watermark on RabbitMQ                                                                                     | `false`                                                 |
| `memoryHighWatermark.type`         | Memory high watermark type. Either `absolute` or `relative`                                                                              | `relative`                                              |
| `memoryHighWatermark.value`        | Memory high watermark value                                                                                                              | `0.4`                                                   |
| `plugins`                          | List of default plugins to enable (should only be altered to remove defaults; for additional plugins use `extraPlugins`)                 | `rabbitmq_management rabbitmq_peer_discovery_k8s`       |
| `communityPlugins`                 | List of custom plugins (URLs) to be downloaded during container initialization                                                           | `nil`                                                   |
| `hostAliases`                      | Add deployment host aliases                                                                                                              | `[]`                                                    |
| `extraPlugins`                     | Extra plugins to enable (single string containing a space-separated list)                                                                | `nil`                                                   |
| `clustering.enabled`               | Enable RabbitMQ clustering                                                                                                               | `true`                                                  |
| `clustering.addressType`           | Switch clustering mode. Either `ip` or `hostname`                                                                                        | `hostname`                                              |
| `clustering.rebalance`             | Rebalance master for queues in cluster when new replica is created                                                                       | `false`                                                 |
| `clustering.forceBoot`             | Force boot of an unexpectedly shut down cluster (in an unexpected order).                                                                | `false`                                                 |
| `loadDefinition.enabled`           | Enable loading a RabbitMQ definitions file to configure RabbitMQ                                                                         | `false`                                                 |
| `loadDefinition.existingSecret`    | Existing secret with the load definitions file                                                                                           | `nil`                                                   |
| `command`                          | Override default container command (useful when using custom images)                                                                     | `nil`                                                   |
| `args`                             | Override default container args (useful when using custom images)                                                                        | `nil`                                                   |
| `terminationGracePeriodSeconds`    | TerminationGracePeriodSeconds of container (time in excess of 10 seconds will be spent waiting for synchronization)                      | `120`                                                   |
| `extraEnvVars`                     | Extra environment variables to add to RabbitMQ pods                                                                                      | `[]`                                                    |
| `extraEnvVarsCM`                   | Name of existing ConfigMap containing extra env vars                                                                                     | `nil`                                                   |
| `extraEnvVarsSecret`               | Name of existing Secret containing extra env vars (in case of sensitive data)                                                            | `nil`                                                   |
| `extraContainerPorts`              | Extra ports to be included in container spec, primarily informational                                                                    | `[]`                                                    |
| `configuration`                    | RabbitMQ configuration                                                                                                                   | Check `values.yaml` file                                |
| `extraConfiguration`               | Extra configuration to be appended to RabbitMQ configuration                                                                             | Check `values.yaml` file                                |
| `advancedConfiguration`            | Extra configuration (in classic format)                                                                                                  | Check `values.yaml` file                                |
| `ldap.enabled`                     | Enable LDAP support                                                                                                                      | `false`                                                 |
| `ldap.servers`                     | List of LDAP servers hostnames                                                                                                           | `[]`                                                    |
| `ldap.port`                        | LDAP servers port                                                                                                                        | `389`                                                   |
| `ldap.user_dn_pattern`             | Pattern used to translate the provided username into a value to be used for the LDAP bind                                                | `cn=${username},dc=example,dc=org`                      |
| `ldap.tls.enabled`                 | Enable TLS for LDAP connections (check advancedConfiguration parameter in values.yml)                                                    | `false`                                                 |

### Statefulset parameters

| Parameter                   | Description                                                                               | Default                        |
|-----------------------------|-------------------------------------------------------------------------------------------|--------------------------------|
| `replicaCount`              | Number of RabbitMQ nodes                                                                  | `1`                            |
| `schedulerName`             | Name of the k8s service (other than default)                                              | `nil`                          |
| `podManagementPolicy`       | Pod management policy                                                                     | `OrderedReady`                 |
| `updateStrategyType`        | Update strategy type for the statefulset                                                  | `RollingUpdate`                |
| `rollingUpdatePartition`    | Partition update strategy                                                                 | `nil`                          |
| `statefulsetLabels`         | RabbitMQ statefulset labels                                                               | `{}` (evaluated as a template) |
| `podLabels`                 | RabbitMQ pod labels                                                                       | `{}` (evaluated as a template) |
| `podAnnotations`            | RabbitMQ Pod annotations                                                                  | `{}` (evaluated as a template) |
| `podAffinityPreset`         | Pod affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`       | `""`                           |
| `podAntiAffinityPreset`     | Pod anti-affinity preset. Ignored if `affinity` is set. Allowed values: `soft` or `hard`  | `soft`                         |
| `nodeAffinityPreset.type`   | Node affinity preset type. Ignored if `affinity` is set. Allowed values: `soft` or `hard` | `""`                           |
| `nodeAffinityPreset.key`    | Node label key to match Ignored if `affinity` is set.                                     | `""`                           |
| `nodeAffinityPreset.values` | Node label values to match. Ignored if `affinity` is set.                                 | `[]`                           |
| `nodeSelector`              | Node labels for pod assignment                                                            | `{}` (evaluated as a template) |
| `tolerations`               | Tolerations for pod assignment                                                            | `[]` (evaluated as a template) |
| `topologySpreadConstraints` | Topology Spread Constraints for pod assignment                                            | `{}` (evaluated as a template) |
| `affinity`                  | Affinity for pod assignment                                                               | `{}` (evaluated as a template) |
| `priorityClassName`         | Name of the existing priority class to be used by rabbitmq pods                           | `""`                           |
| `podSecurityContext`        | RabbitMQ pods' Security Context                                                           | `{}`                           |
| `containerSecurityContext`  | RabbitMQ containers' Security Context                                                     | `{}`                           |
| `resources.limits`          | The resources limits for RabbitMQ containers                                              | `{}`                           |
| `resources.requests`        | The requested resources for RabbitMQ containers                                           | `{}`                           |
| `livenessProbe`             | Liveness probe configuration for RabbitMQ                                                 | Check `values.yaml` file       |
| `readinessProbe`            | Readiness probe configuration for RabbitMQ                                                | Check `values.yaml` file       |
| `customLivenessProbe`       | Override default liveness probe                                                           | `nil`                          |
| `customReadinessProbe`      | Override default readiness probe                                                          | `nil`                          |
| `customStartupProbe`        | Define a custom startup probe                                                             | `nil`                          |
| `pdb.create`                | Enable/disable a Pod Disruption Budget creation                                           | `false`                        |
| `pdb.minAvailable`          | Minimum number/percentage of pods that should remain scheduled                            | `nil`                          |
| `pdb.maxUnavailable`        | Maximum number/percentage of pods that may be made unavailable                            | `1`                            |
| `initContainers`            | Add additional init containers to the RabbitMQ pod                                        | `{}` (evaluated as a template) |
| `sidecars`                  | Add additional sidecar containers to the RabbitMQ pod                                     | `{}` (evaluated as a template) |
| `extraVolumeMounts`         | Optionally specify extra list of additional volumeMounts .                                | `{}`                           |
| `extraVolumes`              | Optionally specify extra list of additional volumes .                                     | `{}`                           |
| `extraSecrets`              | Optionally specify extra secrets to be created by the chart.                              | `{}` (evaluated as a template) |

### Exposure parameters

| Parameter                          | Description                                                                       | Default                        |
|------------------------------------|-----------------------------------------------------------------------------------|--------------------------------|
| `service.type`                     | Kubernetes Service type                                                           | `ClusterIP`                    |
| `service.port`                     | Amqp port                                                                         | `5672`                         |
| `service.portName`                 | Amqp service port name                                                            | `amqp`                         |
| `service.tlsPort`                  | Amqp TLS port                                                                     | `5671`                         |
| `service.tlsPortName`              | Amqp TLS service port name                                                        | `amqp-ssl`                     |
| `service.nodePort`                 | Node port override for `amqp` port, if serviceType NodePort or LoadBalancer       | `nil`                          |
| `service.tlsNodePort`              | Node port override for `amqp-ssl` port, if serviceType NodePort or LoadBalancer   | `nil`                          |
| `service.distPort`                 | Erlang distribution server port                                                   | `25672`                        |
| `service.distPortName`             | Erlang distribution service port name                                             | `dist`                         |
| `service.distNodePort`             | Node port override for `dist` port, if serviceType NodePort                       | `nil`                          |
| `service.managerPortEnable`        | Enable the RabbitMQ Manager port                                                  | `true`                         |
| `service.managerPort`              | RabbitMQ Manager port                                                             | `15672`                        |
| `service.managerPortName`          | RabbitMQ Manager service port name                                                | `http-stats`                   |
| `service.managerNodePort`          | Node port override for `http-stats` port, if serviceType NodePort                 | `nil`                          |
| `service.metricsPort`              | RabbitMQ Prometheues metrics port                                                 | `9419`                         |
| `service.metricsPortName`          | RabbitMQ Prometheues metrics service port name                                    | `metrics`                      |
| `service.metricsNodePort`          | Node port override for `metrics` port, if serviceType NodePort                    | `nil`                          |
| `service.epmdPortName`             | EPMD Discovery service port name                                                  | `epmd`                         |
| `service.epmdNodePort`             | Node port override for `epmd` port, if serviceType NodePort                       | `nil`                          |
| `service.extraPorts`               | Extra ports to expose in the service                                              | `[]`                           |
| `service.loadBalancerSourceRanges` | Address(es) that are allowed when service is LoadBalancer                         | `[]`                           |
| `service.loadBalancerIP`           | LoadBalancerIP for the service                                                    | `nil`                          |
| `service.externalIP`               | ExternalIP for the service                                                        | `nil`                          |
| `service.externalTrafficPolicy`    | Enable client source IP preservation                                              | `Cluster`                      |
| `service.labels`                   | Service labels                                                                    | `{}` (evaluated as a template) |
| `service.annotations`              | Service annotations                                                               | `{}` (evaluated as a template) |
| `service.annotationsHeadless`      | Headless service annotations different from regular service                       | `{}` (evaluated as a template) |
| `ingress.enabled`                  | Enable ingress resource for Management console                                    | `false`                        |
| `ingress.path`                     | Path for the default host                                                         | `/`                            |
| `ingress.certManager`              | Add annotations for cert-manager                                                  | `false`                        |
| `ingress.hostname`                 | Default host for the ingress resource                                             | `rabbitmq.local`               |
| `ingress.pathType`                 | Ingress path type                                                                 | `ImplementationSpecific`       |
| `ingress.annotations`              | Ingress annotations                                                               | `[]`                           |
| `ingress.tls`                      | Enable TLS configuration for the hostname defined at `ingress.hostname` parameter | `false`                        |
| `ingress.existingSecret`           | Existing secret for the Ingress TLS certificate                                   | `nil`                          |
| `ingress.extraHosts[0].name`       | Additional hostnames to be covered                                                | `nil`                          |
| `ingress.extraHosts[0].path`       | Additional hostnames to be covered                                                | `nil`                          |
| `ingress.extraTls[0].hosts[0]`     | TLS configuration for additional hostnames to be covered                          | `nil`                          |
| `ingress.extraTls[0].secretName`   | TLS configuration for additional hostnames to be covered                          | `nil`                          |
| `ingress.secrets[0].name`          | TLS Secret Name                                                                   | `nil`                          |
| `ingress.secrets[0].certificate`   | TLS Secret Certificate                                                            | `nil`                          |
| `ingress.secrets[0].key`           | TLS Secret Key                                                                    | `nil`                          |
| `networkPolicy.enabled`            | Enable NetworkPolicy                                                              | `false`                        |
| `networkPolicy.allowExternal`      | Don't require client label for connections                                        | `true`                         |
| `networkPolicy.additionalRules`    | Additional NetworkPolicy rules                                                    | `nil`                          |

### Persistence parameters

| Parameter                   | Description                                                                       | Default                       |
|-----------------------------|-----------------------------------------------------------------------------------|-------------------------------|
| `persistence.enabled`       | Enable RabbitMQ data persistence using PVC                                        | `true`                        |
| `persistence.existingClaim` | Provide an existing `PersistentVolumeClaim`, the value is evaluated as a template | `nil`                         |
| `persistence.storageClass`  | PVC Storage Class for RabbitMQ data volume                                        | `nil`                         |
| `persistence.accessMode`    | PVC Access Mode for RabbitMQ data volume                                          | `ReadWriteOnce`               |
| `persistence.size`          | PVC Storage Request for RabbitMQ data volume                                      | `8Gi`                         |
| `persistence.selector`      | Selector to match an existing Persistent Volume                                   | `{}`(evaluated as a template) |
| `persistence.volumes`       | Additional volumes without creating PVC                                           | `{}`(evaluated as a template) |

### RBAC parameters

| Parameter               | Description                                         | Default                                          |
|-------------------------|-----------------------------------------------------|--------------------------------------------------|
| `serviceAccount.create` | Enable creation of ServiceAccount for RabbitMQ pods | `true`                                           |
| `serviceAccount.name`   | Name of the created serviceAccount                  | Generated using the `rabbitmq.fullname` template |
| `rbac.create`           | Weather to create & use RBAC resources or not       | `true`                                           |

### Volume Permissions parameters

| Parameter                              | Description                                                                                                          | Default                                                 |
|----------------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------|
| `volumePermissions.enabled`            | Enable init container that changes the owner and group of the persistent volume(s) mountpoint to `runAsUser:fsGroup` | `false`                                                 |
| `volumePermissions.image.registry`     | Init container volume-permissions image registry                                                                     | `docker.io`                                             |
| `volumePermissions.image.repository`   | Init container volume-permissions image name                                                                         | `bitnami/bitnami-shell`                                 |
| `volumePermissions.image.tag`          | Init container volume-permissions image tag                                                                          | `"10"`                                                  |
| `volumePermissions.image.pullPolicy`   | Init container volume-permissions image pull policy                                                                  | `Always`                                                |
| `volumePermissions.image.pullSecrets`  | Specify docker-registry secret names as an array                                                                     | `[]` (does not add image pull secrets to deployed pods) |
| `volumePermissions.resources.limits`   | Init container volume-permissions resource  limits                                                                   | `{}`                                                    |
| `volumePermissions.resources.requests` | Init container volume-permissions resource  requests                                                                 | `{}`                                                    |

### Metrics parameters

| Parameter                                 | Description                                                                            | Default                                                      |
|-------------------------------------------|----------------------------------------------------------------------------------------|--------------------------------------------------------------|
| `metrics.enabled`                         | Enable exposing RabbitMQ metrics to be gathered by Prometheus                          | `false`                                                      |
| `metrics.plugins`                         | Plugins to enable Prometheus metrics in RabbitMQ                                       | `rabbitmq_prometheus`                                        |
| `metrics.podAnnotations`                  | Annotations for enabling prometheus to access the metrics endpoint                     | `{prometheus.io/scrape: "true", prometheus.io/port: "9419"}` |
| `metrics.serviceMonitor.enabled`          | Create ServiceMonitor Resource for scraping metrics using PrometheusOperator           | `false`                                                      |
| `metrics.serviceMonitor.namespace`        | Namespace which Prometheus is running in                                               | `monitoring`                                                 |
| `metrics.serviceMonitor.interval`         | Interval at which metrics should be scraped                                            | `30s`                                                        |
| `metrics.serviceMonitor.scrapeTimeout`    | Specify the timeout after which the scrape is ended                                    | `nil`                                                        |
| `metrics.serviceMonitor.relabellings`     | Specify Metric Relabellings to add to the scrape endpoint                              | `nil`                                                        |
| `metrics.serviceMonitor.honorLabels`      | honorLabels chooses the metric's labels on collisions with target labels.              | `false`                                                      |
| `metrics.serviceMonitor.targetLabels`     | Used to keep given service's labels in  target                                         | `[]`                                                         |
| `metrics.serviceMonitor.podTargetLabels`  | Used to keep given pod's labels in target                                              | `[]`                                                         |
| `metrics.serviceMonitor.additionalLabels` | Used to pass Labels that are required by the Installed Prometheus Operator             | `{}`                                                         |
| `metrics.prometheusRule.enabled`          | Set this to true to create prometheusRules for Prometheus operator                     | `false`                                                      |
| `metrics.prometheusRule.additionalLabels` | Additional labels that can be used so prometheusRules will be discovered by Prometheus | `{}`                                                         |
| `metrics.prometheusRule.namespace`        | namespace where prometheusRules resource should be created                             | `monitoring`                                                 |
| `metrics.prometheusRule.rules`            | Rules to be created, check values for an example.                                      | `[]`                                                         |

The above parameters map to the env variables defined in [bitnami/rabbitmq](http://github.com/bitnami/bitnami-docker-rabbitmq). For more information please refer to the [bitnami/rabbitmq](http://github.com/bitnami/bitnami-docker-rabbitmq) image documentation.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install my-release \
  --set auth.username=admin,auth.password=secretpassword,auth.erlangCookie=secretcookie \
    bitnami/rabbitmq
```

The above command sets the RabbitMQ admin username and password to `admin` and `secretpassword` respectively. Additionally the secure erlang cookie is set to `secretcookie`.

> NOTE: Once this chart is deployed, it is not possible to change the application's access credentials, such as usernames or passwords, using Helm. To change these application credentials after deployment, delete any persistent volumes (PVs) used by the chart and re-deploy it, or use the application's built-in administrative tools if available.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install my-release -f values.yaml bitnami/rabbitmq
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Configuration and installation details

### [Rolling vs Immutable tags](https://docs.bitnami.com/containers/how-to/understand-rolling-tags-containers/)

It is strongly recommended to use immutable tags in a production environment. This ensures your deployment does not change automatically if the same tag is updated with a different image.

Bitnami will release a new chart updating its containers if a new version of the main container, significant changes, or critical vulnerabilities exist.

### Set pod affinity

This chart allows you to set your custom affinity using the `affinity` parameter. Find more information about Pod's affinity in the [kubernetes documentation](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity).

As an alternative, you can use of the preset configurations for pod affinity, pod anti-affinity, and node affinity available at the [bitnami/common](https://github.com/bitnami/charts/tree/master/bitnami/common#affinities) chart. To do so, set the `podAffinityPreset`, `podAntiAffinityPreset`, or `nodeAffinityPreset` parameters.

### Scale horizontally

To horizontally scale this chart once it has been deployed, two options are available:

- Use the `kubectl scale` command.
- Upgrade the chart modifying the `replicaCount` parameter.

> NOTE: It is mandatory to specify the password and Erlang cookie that was set the first time the chart was installed when upgrading the chart.

When scaling down the solution, unnecessary RabbitMQ nodes are automatically stopped, but they are not removed from the cluster. You need to manually remove them by running the `rabbitmqctl forget_cluster_node` command.

Refer to the chart documentation for [more information on scaling the Rabbit cluster horizontally](https://docs.bitnami.com/kubernetes/infrastructure/rabbitmq/administration/scale-out/).

### Enable TLS support

To enable TLS support, first generate the certificates as described in the [RabbitMQ documentation for SSL certificate generation](https://www.rabbitmq.com/ssl.html#automated-certificate-generation).

Once the certificates are generated, you have two alternatives:

* Create a secret with the certificates and associate the secret when deploying the chart
* Include the certificates in the *values.yaml* file when deploying the chart

Set the *auth.tls.failIfNoPeerCert* parameter to *false* to allow a TLS connection if the client fails to provide a certificate.

Set the *auth.tls.sslOptionsVerify* to *verify_peer* to force a node to perform peer verification. When set to *verify_none*, peer verification will be disabled and certificate exchange won't be performed.

Refer to the chart documentation for [more information and examples of enabling TLS and using Let's Encrypt certificates](https://docs.bitnami.com/kubernetes/infrastructure/rabbitmq/administration/enable-tls/).

### Load custom definitions

It is possible to [load a RabbitMQ definitions file to configure RabbitMQ](http://www.rabbitmq.com/management.html#load-definitions).

Because definitions may contain RabbitMQ credentials, [store the JSON as a Kubernetes secret](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-files-from-a-pod). Within the secret's data, choose a key name that corresponds with the desired load definitions filename (i.e. `load_definition.json`) and use the JSON object as the value.

Next, specify the `load_definitions` property as an `extraConfiguration` pointing to the load definition file path within the container (i.e. `/app/load_definition.json`) and set `loadDefinition.enable` to `true`. Any load definitions specified will be available within in the container at `/app`.

> NOTE: Loading a definition will take precedence over any configuration done through [Helm values](#parameters).

If needed, you can use `extraSecrets` to let the chart create the secret for you. This way, you don't need to manually create it before deploying a release. These secrets can also be templated to use supplied chart values.

Refer to the chart documentation for [more information and configuration examples of loading custom definitions](https://docs.bitnami.com/kubernetes/infrastructure/rabbitmq/configuration/load-files/).

### Configure LDAP support

LDAP support can be enabled in the chart by specifying the `ldap.*` parameters while creating a release. Refer to the chart documentation for [more information and a configuration example](https://docs.bitnami.com/kubernetes/infrastructure/rabbitmq/configuration/configure-ldap/).

### Configure memory high watermark

It is possible to configure a memory high watermark on RabbitMQ to define [memory thresholds](https://www.rabbitmq.com/memory.html#threshold) using the `memoryHighWatermark.*` parameters. To do so, you have two alternatives:

* Set an absolute limit of RAM to be used on each RabbitMQ node, as shown in the configuration example below:

```
memoryHighWatermark.enabled="true"
memoryHighWatermark.type="absolute"
memoryHighWatermark.value="512MB"
```

* Set a relative limit of RAM to be used on each RabbitMQ node. To enable this feature,  define the memory limits at pod level too. An example configuration is shown below:

```
memoryHighWatermark.enabled="true"
memoryHighWatermark.type="relative"
memoryHighWatermark.value="0.4"
resources.limits.memory="2Gi"
```

### Add extra environment variables

In case you want to add extra environment variables (useful for advanced operations like custom init scripts), you can use the `extraEnvVars` property.

```yaml
extraEnvVars:
  - name: LOG_LEVEL
    value: error
```

Alternatively, you can use a ConfigMap or a Secret with the environment variables. To do so, use the `.extraEnvVarsCM` or the `extraEnvVarsSecret` properties.

### Use plugins

The Bitnami Docker RabbitMQ image ships a set of plugins by default. By default, this chart enables `rabbitmq_management` and `rabbitmq_peer_discovery_k8s` since they are required for RabbitMQ to work on K8s.

To enable extra plugins, set the `extraPlugins` parameter with the list of plugins you want to enable. In addition to this, the `communityPlugins` parameter can be used to specify a list of URLs (separated by spaces) for custom plugins for RabbitMQ.

Refer to the chart documentation for [more information on using RabbitMQ plugins](https://docs.bitnami.com/kubernetes/infrastructure/rabbitmq/configuration/use-plugins/).

### Recover the cluster from complete shutdown

> IMPORTANT: Some of these procedures can lead to data loss. Always make a backup beforehand.

The RabbitMQ cluster is able to support multiple node failures but, in a situation in which all the nodes are brought down at the same time, the cluster might not be able to self-recover.

This happens if the pod management policy of the statefulset is not `Parallel` and the last pod to be running wasn't the first pod of the statefulset. If that happens, update the pod management policy to recover a healthy state:

```console
$ kubectl delete statefulset STATEFULSET_NAME --cascade=false
$ helm upgrade RELEASE_NAME bitnami/rabbitmq \
    --set podManagementPolicy=Parallel \
    --set replicaCount=NUMBER_OF_REPLICAS \
    --set auth.password=PASSWORD \
    --set auth.erlangCookie=ERLANG_COOKIE
```

For a faster resyncronization of the nodes, you can temporarily disable the readiness probe by setting `readinessProbe.enabled=false`. Bear in mind that the pods will be exposed before they are actually ready to process requests.

If the steps above don't bring the cluster to a healthy state, it could be possible that none of the RabbitMQ nodes think they were the last node to be up during the shutdown. In those cases, you can force the boot of the nodes by specifying the `clustering.forceBoot=true` parameter (which will execute [`rabbitmqctl force_boot`](https://www.rabbitmq.com/rabbitmqctl.8.html#force_boot) in each pod):

```console
$ helm upgrade RELEASE_NAME bitnami/rabbitmq \
    --set podManagementPolicy=Parallel \
    --set clustering.forceBoot=true \
    --set replicaCount=NUMBER_OF_REPLICAS \
    --set auth.password=PASSWORD \
    --set auth.erlangCookie=ERLANG_COOKIE
```

More information: [Clustering Guide: Restarting](https://www.rabbitmq.com/clustering.html#restarting).

### Known issues

- Changing the password through RabbitMQ's UI can make the pod fail due to the default liveness probes. If you do so, remember to make the chart aware of the new password. Updating the default secret with the password you set through RabbitMQ's UI will automatically recreate the pods. If you are using your own secret, you may have to manually recreate the pods.

## Persistence

The [Bitnami RabbitMQ](https://github.com/bitnami/bitnami-docker-rabbitmq) image stores the RabbitMQ data and configurations at the `/opt/bitnami/rabbitmq/var/lib/rabbitmq/` path of the container.

The chart mounts a [Persistent Volume](http://kubernetes.io/docs/user-guide/persistent-volumes/) at this location. By default, the volume is created using dynamic volume provisioning. An existing PersistentVolumeClaim can also be defined.

### Use existing PersistentVolumeClaims

1. Create the PersistentVolume
1. Create the PersistentVolumeClaim
1. Install the chart

```bash
$ helm install my-release --set persistence.existingClaim=PVC_NAME bitnami/rabbitmq
```

### Adjust permissions of the persistence volume mountpoint

As the image runs as non-root by default, it is necessary to adjust the ownership of the persistent volume so that the container can write data into it.

By default, the chart is configured to use Kubernetes Security Context to automatically change the ownership of the volume. However, this feature does not work in all Kubernetes distributions.
As an alternative, this chart supports using an `initContainer` to change the ownership of the volume before mounting it in the final destination.

You can enable this `initContainer` by setting `volumePermissions.enabled` to `true`.

### Configure the default user/vhost

If you want to create default user/vhost and set the default permission. you can use `extraConfiguration`:

```yaml
auth:
  username: default-user
extraConfiguration: |-
  default_vhost = default-vhost
  default_permissions.configure = .*
  default_permissions.read = .*
  default_permissions.write = .*
```

## Troubleshooting

Find more information about how to deal with common errors related to Bitnami’s Helm charts in [this troubleshooting guide](https://docs.bitnami.com/general/how-to/troubleshoot-helm-chart-issues).

## Upgrading

It's necessary to set the `auth.password` and `auth.erlangCookie` parameters when upgrading for readiness/liveness probes to work properly. When you install this chart for the first time, some notes will be displayed providing the credentials you must use under the 'Credentials' section. Please note down the password and the cookie, and run the command below to upgrade your chart:

```bash
$ helm upgrade my-release bitnami/rabbitmq --set auth.password=[PASSWORD] --set auth.erlangCookie=[RABBITMQ_ERLANG_COOKIE]
```

| Note: you need to substitute the placeholders [PASSWORD] and [RABBITMQ_ERLANG_COOKIE] with the values obtained in the installation notes.

### To 8.0.0

[On November 13, 2020, Helm v2 support was formally finished](https://github.com/helm/charts#status-of-the-project), this major version is the result of the required changes applied to the Helm Chart to be able to incorporate the different features added in Helm v3 and to be consistent with the Helm project itself regarding the Helm v2 EOL.

[Learn more about this change and related upgrade considerations](https://docs.bitnami.com/kubernetes/infrastructure/rabbitmq/administration/upgrade-helm3/).

### To 7.0.0

- Several parameters were renamed or disappeared in favor of new ones on this major version:
  - `replicas` is renamed to `replicaCount`.
  - `securityContext.*` is deprecated in favor of `podSecurityContext` and `containerSecurityContext`.
  - Authentication parameters were reorganized under the `auth.*` parameter:
    - `rabbitmq.username`, `rabbitmq.password`, and `rabbitmq.erlangCookie` are now `auth.username`, `auth.password`, and `auth.erlangCookie` respectively.
    - `rabbitmq.tls.*` parameters are now under `auth.tls.*`.
  - Parameters prefixed with `rabbitmq.` were renamed removing the prefix. E.g. `rabbitmq.configuration` -> renamed to `configuration`.
  - `rabbitmq.rabbitmqClusterNodeName` is deprecated.
  - `rabbitmq.setUlimitNofiles` is deprecated.
  - `forceBoot.enabled` is renamed to `clustering.forceBoot`.
  - `loadDefinition.secretName` is renamed to `loadDefinition.existingSecret`.
  - `metics.port` is remamed to `service.metricsPort`.
  - `service.extraContainerPorts` is renamed to `extraContainerPorts`.
  - `service.nodeTlsPort` is renamed to `service.tlsNodePort`.
  - `podDisruptionBudget` is deprecated in favor of `pdb.create`, `pdb.minAvailable`, and `pdb.maxUnavailable`.
  - `rbacEnabled` -> deprecated in favor of `rbac.create`.
  - New parameters: `serviceAccount.create`, and `serviceAccount.name`.
  - New parameters: `memoryHighWatermark.enabled`, `memoryHighWatermark.type`, and `memoryHighWatermark.value`.
- Chart labels and Ingress configuration were adapted to follow the Helm charts best practices.
- Initialization logic now relies on the container.
- This version introduces `bitnami/common`, a [library chart](https://helm.sh/docs/topics/library_charts/#helm) as a dependency. More documentation about this new utility could be found [here](https://github.com/bitnami/charts/tree/master/bitnami/common#bitnami-common-library-chart). Please, make sure that you have updated the chart dependencies before executing any upgrade.

Consequences:

- Backwards compatibility is not guaranteed.
- Compatibility with non Bitnami images is not guaranteed anymore.

### To 6.0.0

This new version updates the RabbitMQ image to a [new version based on bash instead of node.js](https://github.com/bitnami/bitnami-docker-rabbitmq#3715-r18-3715-ol-7-r19). However, since this Chart overwrites the container's command, the changes to the container shouldn't affect the Chart. To upgrade, it may be needed to enable the `fastBoot` option, as it is already the case from upgrading from 5.X to 5.Y.

### To 5.0.0

This major release changes the clustering method from `ip` to `hostname`.
This change is needed to fix the persistence. The data dir will now depend on the hostname which is stable instead of the pod IP that might change.

> IMPORTANT: Note that if you upgrade from a previous version you will lose your data.

### To 3.0.0

Backwards compatibility is not guaranteed unless you modify the labels used on the chart's deployments.
Use the workaround below to upgrade from versions previous to 3.0.0. The following example assumes that the release name is rabbitmq:

```console
$ kubectl delete statefulset rabbitmq --cascade=false
```

## Bitnami Kubernetes Documentation

Bitnami Kubernetes documentation is available at [https://docs.bitnami.com/](https://docs.bitnami.com/). You can find there the following resources:

- [Documentation for RabbitMQ Helm chart](https://docs.bitnami.com/kubernetes/infrastructure/rabbitmq/)
- [Get Started with Kubernetes guides](https://docs.bitnami.com/kubernetes/)
- [Bitnami Helm charts documentation](https://docs.bitnami.com/kubernetes/apps/)
- [Kubernetes FAQs](https://docs.bitnami.com/kubernetes/faq/)
- [Kubernetes Developer guides](https://docs.bitnami.com/tutorials/)
