# coturn

![Version: 4.2.2](https://img.shields.io/badge/Version-4.2.2-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 4.6.2](https://img.shields.io/badge/AppVersion-4.6.2-informational?style=flat-square)

A Helm chart to deploy coturn

**Homepage:** <https://github.com/jessebot/coturn-chart>

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| jessebot | <jessebot@linux.com> | <https://github.com/jessebot/> |

## Source Code

* <https://github.com/jessebot/coturn-chart>

## Requirements

| Repository | Name | Version |
|------------|------|---------|
| oci://registry-1.docker.io/bitnamicharts | postgresql | 12.9.0 |

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| certificate.enabled | bool | `false` | Enables auto issuing certificates over cert-manager certificates https://cert-manager.io/docs/concepts/certificate/ |
| certificate.issuerName | string | `"letsencrypt-staging"` | name of cert-manager issuer to use for cert generation. change to production issuer when you're stable |
| certificate.secret | string | `"turn-tls"` | name of secret to create for ssl cert |
| coturn.auth.existingSecret | string | `""` | existing secret with keys username/password for coturn |
| coturn.auth.password | string | `""` | password for the main user of the turn server |
| coturn.auth.secretKeys.password | string | `"password"` | key in existing secret for turn server user's password |
| coturn.auth.secretKeys.username | string | `"username"` | key in existing secret for turn server user |
| coturn.auth.username | string | `"coturn"` | username for the main user of the turn server |
| coturn.extraTurnserverConfiguration | string | `"verbose\n"` | extra configuration for turnserver.conf |
| coturn.listeningIP | string | `"0.0.0.0"` | coturn's listening IP address |
| coturn.logFile | string | `"stdout"` | set the logfile. Defaults to stdout for use with kubectl logs |
| coturn.ports.listening | int | `3478` | insecure listening port |
| coturn.ports.max | int | `65535` | maximum ephemeral port for coturn |
| coturn.ports.min | int | `49152` | minimum ephemeral port for coturn |
| coturn.ports.tlsListening | int | `5349` | secure listening port |
| coturn.realm | string | `"turn.example.com"` | hostname for the coturn server realm |
| externalDatabase.database | string | `""` | database to create, ignored if existingSecret is passed in |
| externalDatabase.enabled | bool | `false` | enables the use of postgresql instead of the default sqlite to use the bundled subchart, enable this, and postgresql.enable |
| externalDatabase.existingSecret | string | `""` | name of existing Secret to use for postgresql credentials |
| externalDatabase.hostname | string | `""` | required if externalDatabase.enabled: true and postgresql.enabled: false |
| externalDatabase.password | string | `""` | password for database, ignored if existingSecret is passed in |
| externalDatabase.secretKeys.database | string | `""` | key in existing Secret to use for the database name |
| externalDatabase.secretKeys.hostname | string | `""` | key in existing Secret to use for the db's hostname |
| externalDatabase.secretKeys.password | string | `""` | key in existing Secret to use for db user's password |
| externalDatabase.secretKeys.username | string | `""` | key in existing Secret to use for the db user |
| externalDatabase.type | string | `"postgresql"` | Currently only postgresql is supported. mysql coming soon |
| externalDatabase.username | string | `""` | username for database, ignored if existingSecret is passed in |
| image.pullPolicy | string | `"IfNotPresent"` | image pull policy, set to Always if using image.tag: latest |
| image.repository | string | `"coturn/coturn"` | container registry and repo for coturn docker image |
| image.tag | string | `""` | docker tag for coturn server |
| labels | object | `{"component":"coturn"}` | Coturn specific labels |
| nameOverride | string | `""` | different name for the helm release |
| persistence.accessMode | string | `"ReadWriteOnce"` | access mode for the PVC, ignored if persistence.existingClaim passed in |
| persistence.annotations | object | `{}` | annotations for the PVC, ignored if persistence.existingClaim passed in |
| persistence.existingClaim | string | `""` | existing PVC to use instead of creating one on the fly |
| persistence.size | string | `"1Mi"` | size of the PVC, ignored if persistence.existingClaim passed in |
| persistence.storageClass | string | `""` | storageClass for the PVC, ignored if persistence.existingClaim passed in |
| postgresql.enabled | bool | `false` | enables bitnami postgresql subchart, you can disable to use external db |
| postgresql.global.postgresql.auth.database | string | `"coturn"` | database to create, ignored if existingSecret is passed in |
| postgresql.global.postgresql.auth.existingSecret | string | `""` | name of existing Secret to use for postgresql credentials |
| postgresql.global.postgresql.auth.password | string | `""` | password for db, autogenerated if empty & existingSecret empty |
| postgresql.global.postgresql.auth.secretKeys.adminPasswordKey | string | `"postgresPassword"` | key in existing Secret to use for postgres admin user's password |
| postgresql.global.postgresql.auth.secretKeys.database | string | `"database"` | key in existingSecret for database to create |
| postgresql.global.postgresql.auth.secretKeys.hostname | string | `"hostname"` | key in existingSecret for database to create |
| postgresql.global.postgresql.auth.secretKeys.userPasswordKey | string | `"password"` | key in existing Secret to use for coturn user's password |
| postgresql.global.postgresql.auth.secretKeys.username | string | `"username"` | key in exsiting Secret to use for the coturn user |
| postgresql.global.postgresql.auth.username | string | `"coturn"` | username for database, ignored if existingSecret is passed in |
| replicas | int | `1` |  |
| resources | object | `{}` | ref: kubernetes.io/docs/concepts/configuration/manage-resources-containers |
| securityContext.allowPrivilegeEscalation | bool | `true` | allow priviledged access |
| securityContext.capabilities.add | list | `["NET_BIND_SERVICE"]` | linux cabilities to allow for the coturn k8s pod |
| securityContext.capabilities.drop | list | `["ALL"]` | linux cabilities to disallow for the coturn k8s pod |
| securityContext.fsGroup | int | `1000` | all processes of the container are also part of the supplementary groupID |
| securityContext.readOnlyRootFilesystem | bool | `false` | allow modificatin to root filesystem |
| securityContext.runAsGroup | int | `1000` | for all Containers in the Pod, all processes run w/ this GroupID |
| securityContext.runAsUser | int | `1000` | for all Containers in the Pod, all processes run w/ this userID |
| service.externalTrafficPolicy | string | `""` | I don't actually know what this is 🤔 open a PR if you know    was originally "Local" |
| service.type | string | `"ClusterIP"` | The type of service to deploy for routing Coturn traffic.   ClusterIP: Recommended for DaemonSet configurations. This will create a              standard Kubernetes service for Coturn within the cluster.              No external networking will be configured as the DaemonSet              will handle binding to each Node's host networking    NodePort:  Recommended for Deployment configurations. This will open              TURN ports on every node and route traffic on these ports to              the Coturn pods. You will need to make sure your cloud              provider supports the cluster config setting,              apiserver.service-node-port-range, as this range must contain              the ports defined above for the service to be created.    LoadBalancer: This was what was originally set for this chart in the                 upstream of this fork, but with no details |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.11.0](https://github.com/norwoodj/helm-docs/releases/v1.11.0)
