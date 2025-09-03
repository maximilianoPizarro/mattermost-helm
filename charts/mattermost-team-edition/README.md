# Mattermost Team Edition OpenShift

<link rel="icon" href="https://raw.githubusercontent.com/maximilianoPizarro/botpress-helm-chart/main/favicon-152.ico" type="image/x-icon" >
<p align="left">
<img src="https://img.shields.io/badge/redhat-CC0000?style=for-the-badge&logo=redhat&logoColor=white" alt="Redhat">
<img src="https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white" alt="kubernetes">
<img src="https://img.shields.io/badge/helm-0db7ed?style=for-the-badge&logo=helm&logoColor=white" alt="Helm">
<img src="https://img.shields.io/badge/shell_script-%23121011.svg?style=for-the-badge&logo=gnu-bash&logoColor=white" alt="shell">
<a href="https://www.linkedin.com/in/maximiliano-gregorio-pizarro-consultor-it"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white" alt="linkedin" /></a>
<a href="https://artifacthub.io/packages/search?repo=mattermost-team-edition"><img src="https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/mattermost-team-edition" alt="Artifact Hub" /></a>
</p>

Mattermost Team Edition is an open-source, self-hosted messaging platform for teams. It offers a free, community-driven solution for secure, private collaboration. Features include group and direct messaging, file sharing, and powerful search, giving your team a modern communication hub.

[Mattermost](https://mattermost.com/) is a hybrid cloud enterprise messaging workspace that brings your messaging and tools together to get more done, faster.

## TL;DR;

```bash
$ oc new-project mattermost && oc adm policy add-scc-to-user anyuid system:serviceaccount:mattermost:default
```

```bash
$ helm install mattermost-team-edition/mattermost-team-edition \
  --set mysql.mysqlUser=sampleUser \
  --set mysql.mysqlPassword=samplePassword \
```

```bash
$ oc expose service mattermost-team-edition --port=mattermost-team-edition --hostname=mattermost.apps.rosa.oxkgt-r6dtf-xxq.l9yc.p3.openshiftapps.com --name=mattermost-team-edition --path='/' --termination=edge --insecure-policy=Redirect
```

Replace mattermost.apps.rosa.oxkgt-r6dtf-xxq.l9yc.p3.openshiftapps.com with your OpenShift cluster's hostname.

## Introduction

This chart creates a [Mattermost Team Edition](https://mattermost.com/) deployment on a [Kubernetes](http://kubernetes.io)
cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

Based on your prerequisites list, here's a revised version that is more accurate, up-to-date, and clear for a modern Kubernetes/OpenShift environment:

* **Kubernetes 1.20+**: This is a clear and direct requirement.
* **Helm v3**: This is the current standard. Helm v2 and its Tiller component are now deprecated and should not be used. 
* **Cluster access with `cluster-admin` role**: A user with `cluster-admin` privileges is required to install applications that need to create cluster-wide resources. This is a best practice for a first-time setup or for installing complex applications. For standard installations, namespace-level permissions are often sufficient, but `cluster-admin` is a safe bet to avoid permission-related errors.
* **OpenShift Cluster v4.14+**: This is a direct statement of the required platform.

---

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ oc new-project mattermost && oc adm policy add-scc-to-user anyuid system:serviceaccount:mattermost:default
```

 **Helm v3 command**
```bash
$ helm install my-release mattermost-team-edition/mattermost-team-edition
```

The command deploys Mattermost on the Kubernetes cluster in the default configuration. The [configuration](#configuration)
section lists the parameters that can be configured during installation.

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following table lists the configurable parameters of the Mattermost Team Edition chart and their default values.

Parameter                             | Description                                                                                     | Default
---                                   | ---                                                                                             | ---
`configJSON`                          | The `config.json` configuration to be used by the mattermost server. The values you provide will by using Helm's merging behavior override individual default values only. See the [example configuration](#example-configuration) and the [Mattermost documentation](https://docs.mattermost.com/administration/config-settings.html) for details. |  See `configJSON` in [values.yaml](https://github.com/helm/charts/blob/master/stable/mattermost-team-edition/values.yaml)
`containerSecurityContext`            | Security context for the container                                                              | `null`
`image.repository`                    | Container image repository                                                                      | `mattermost/mattermost-team-edition`
`image.tag`                           | Container image tag                                                                             | `5.39.0`
`image.imagePullPolicy`               | Container image pull policy                                                                     | `IfNotPresent`
`initContainerImage.repository`       | Init container image repository                                                                 | `appropriate/curl`
`initContainerImage.tag`              | Init container image tag                                                                        | `latest`
`initContainerImage.imagePullPolicy`  | Container image pull policy                                                                     | `IfNotPresent`
`revisionHistoryLimit`                | How many old ReplicaSets for Mattermost Deployment you want to retain                           | `1`
`ingress.enabled`                     | If `true`, an ingress is created                                                                | `false`
`ingress.hosts`                       | A list of ingress hosts                                                                         | `[mattermost.example.com]`
`ingress.tls`                         | A list of [ingress tls](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls) items | `[]`
`mysql.enabled`                       | Enables deployment of a mysql server                                                            | `true`
`mysql.mysqlRootPassword`             | Root Password for Mysql (Optional)                                                              | ""
`mysql.mysqlUser`                     | Username for Mysql (Required)                                                                   | ""
`mysql.mysqlPassword`                 | User Password for Mysql (Required)                                                              | ""
`mysql.mysqlDatabase`                 | Database name (Required)                                                                        | "mattermost"
`externalDB.enabled`                  | Enables use of an preconfigured external database server                                        | `false`
`externalDB.externalDriverType`       | `"postgres"` or `"mysql"`                                                                       | ""
`externalDB.externalConnectionString` | See the section about [external databases](#External-Databases).                                | ""
`extraPodAnnotations`                 | Extra pod annotations to be used in the deployments                                             | `[]`
`extraEnvVars`                        | Extra environments variables to be used in the deployments                                      | `[]`
`extraInitContainers`                 | Additional init containers                                                                      | `[]`
`securityContext`                     | Security context for the pod                                                                    | `null`
`service.annotations`                 | Service annotations                                                                             | `{}`
`service.loadBalancerIP`              | A user-specified IP address for service type LoadBalancer to use as External IP (if supported)  | `nil`
`service.loadBalancerSourceRanges`    | list of IP CIDRs allowed access to load balancer (if supported)                                 | `[]`
`serviceAccount.create`               | Enables creation and use of a service account for the mattermost pod                            | `false`
`serviceAccount.name`                 | Name of the service account to create and use                                                   | ""
`serviceAccount.annotations`          | The service account annotations                                                                 | `{}`

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install --name my-release \
  --set image.tag=5.35.3 \
  --set mysql.mysqlUser=sampleUser \
  --set mysql.mysqlPassword=samplePassword \
  mattermost-team-edition/mattermost-team-edition
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name my-release -f values.yaml mattermost/mattermost-team-edition
```

### Example configuration

A basic example of a `.yaml` file with values that could be passed to the `helm`
command with the `-f` or `--values` flag to get started.

```yaml
ingress:
  enabled: true
  hosts:
    - mattermost.example.com

configJSON:
  ServiceSettings:
    SiteURL: "https://mattermost.example.com"
  TeamSettings:
    SiteName: "Mattermost on Example.com"
```

### External Databases
There is an option to use external database services (PostgreSQL or MySQL) for your Mattermost installation.
If you use an external Database you will need to disable the MySQL chart in the `values.yaml`

```yaml
mysql:
  enabled: false
```

#### PostgreSQL
To use an external **PostgreSQL**, You need to set Mattermost **externalDB** config

**IMPORTANT:** Make sure the DB is already created before deploying Mattermost services

```yaml
externalDB:
  enabled: true
  externalDriverType: "postgres"
  externalConnectionString: "<USERNAME>:<PASSWORD>@<HOST>:5432/<DATABASE_NAME>?sslmode=disable&connect_timeout=10"
```

#### MySQL
To use an external **MySQL**, You need to set Mattermost **externalDB** config

**IMPORTANT:** Make sure the DB is already created before deploying Mattermost services

```yaml
externalDB:
  enabled: true
  externalDriverType: "mysql"
  externalConnectionString: "<USERNAME>:<PASSWORD>@tcp(<HOST>:3306)/<DATABASE_NAME>?charset=utf8mb4,utf8&readTimeout=30s&writeTimeout=30s"
```

#### Expose extra ports
To use plugins that require extra ports to be exposed, you can use the following config

```yaml
extraPorts:
    - name: plugin-name
      port: 8585
      protocol: TCP
```

### Local development

For local testing use [minikube](https://github.com/kubernetes/minikube)

Create local cluster using with specified Kubernetes version (e.g. `1.15.6`)

```bash
$ minikube start --kubernetes-version v1.15.6
```

Initialize helm

```bash
$ helm init
```
Above command is not required for Helm v3

Get dependencies

```bash
$ helm dependency update
```

Perform local installation

```bash
$ helm install . \
    --set image.tag=5.35.3 \
    --set mysql.mysqlUser=sampleUser \
    --set mysql.mysqlPassword=samplePassword
```

 **Helm v3 command**
```bash
$ helm install . \
    --generate-name \
    --set image.tag=5.35.3 \
    --set mysql.mysqlUser=sampleUser \
    --set mysql.mysqlPassword=samplePassword
```

#### Limitations

For the Team Edition you can have just one replica running.
