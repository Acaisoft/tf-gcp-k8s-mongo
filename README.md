# Mongo DB Terraform module

Terraform module to deploy [MongoDB] in k8s cluster on GCP.  
Module uses [stable/mongodb] chart.

## Configuration
The following table lists the configurable parameters of the MongoDB chart and their default values.

| Parameter                                          | Description                                                                                  | Default                                                 |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| `global.imageRegistry`                             | Global Docker image registry                                                                 | `nil`                                                   |
| `image.registry`                                   | MongoDB image registry                                                                       | `docker.io`                                             |
| `image.repository`                                 | MongoDB Image name                                                                           | `bitnami/mongodb`                                       |
| `image.tag`                                        | MongoDB Image tag                                                                            | `{VERSION}`                                             |
| `image.pullPolicy`                                 | Image pull policy                                                                            | `Always`                                                |
| `image.pullSecrets`                                | Specify docker-registry secret names as an array                                             | `[]` (does not add image pull secrets to deployed pods) |
| `image.debug`                                      | Specify if debug logs should be enabled                                                      | `false`                                                 |
| `usePassword`                                      | Enable password authentication                                                               | `true`                                                  |
| `existingSecret`                                   | Existing secret with MongoDB credentials                                                     | `nil`                                                   |
| `mongodbRootPassword`                              | MongoDB admin password                                                                       | `random alphanumeric string (10)`                       |
| `mongodbUsername`                                  | MongoDB custom user                                                                          | `nil`                                                   |
| `mongodbPassword`                                  | MongoDB custom user password                                                                 | `random alphanumeric string (10)`                       |
| `mongodbDatabase`                                  | Database to create                                                                           | `nil`                                                   |
| `mongodbEnableIPv6`                                | Switch to enable/disable IPv6 on MongoDB                                                     | `true`                                                  |
| `mongodbSystemLogVerbosity`                        | MongoDB systen log verbosity level                                                           | `0`                                                     |
| `mongodbDisableSystemLog`                          | Whether to disable MongoDB system log or not                                                 | `false`                                                 |
| `mongodbExtraFlags`                                | MongoDB additional command line flags                                                        | []                                                      |
| `service.annotations`                              | Kubernetes service annotations                                                               | `{}`                                                    |
| `service.type`                                     | Kubernetes Service type                                                                      | `ClusterIP`                                             |
| `service.clusterIP`                                | Static clusterIP or None for headless services                                               | `nil`                                                   |
| `service.nodePort`                                 | Port to bind to for NodePort service type                                                    | `nil`                                                   |
| `port`                                             | MongoDB service port                                                                         | `27017`                                                 |
| `replicaSet.enabled`                               | Switch to enable/disable replica set configuration                                           | `false`                                                 |
| `replicaSet.name`                                  | Name of the replica set                                                                      | `rs0`                                                   |
| `replicaSet.useHostnames`                          | Enable DNS hostnames in the replica set config                                               | `true`                                                  |
| `replicaSet.key`                                   | Key used for authentication in the replica set                                               | `nil`                                                   |
| `replicaSet.replicas.secondary`                    | Number of secondary nodes in the replica set                                                 | `1`                                                     |
| `replicaSet.replicas.arbiter`                      | Number of arbiter nodes in the replica set                                                   | `1`                                                     |
| `replicaSet.pdb.minAvailable.primary`              | PDB for the MongoDB Primary nodes                                                            | `1`                                                     |
| `replicaSet.pdb.minAvailable.secondary`            | PDB for the MongoDB Secondary nodes                                                          | `1`                                                     |
| `replicaSet.pdb.minAvailable.arbiter`              | PDB for the MongoDB Arbiter nodes                                                            | `1`                                                     |
| `podAnnotations`                                   | Annotations to be added to pods                                                              | {}                                                      |
| `podLabels`                                        | Additional labels for the pod(s).                                                            | {}                                                      |
| `resources`                                        | Pod resources                                                                                | {}                                                      |
| `priorityClassName`                                | Pod priority class name                                                                      | ``                                                      |
| `nodeSelector`                                     | Node labels for pod assignment                                                               | {}                                                      |
| `affinity`                                         | Affinity for pod assignment                                                                  | {}                                                      |
| `tolerations`                                      | Toleration labels for pod assignment                                                         | {}                                                      |
| `securityContext.enabled`                          | Enable security context                                                                      | `true`                                                  |
| `securityContext.fsGroup`                          | Group ID for the container                                                                   | `1001`                                                  |
| `securityContext.runAsUser`                        | User ID for the container                                                                    | `1001`                                                  |
| `persistence.enabled`                              | Use a PVC to persist data                                                                    | `true`                                                  |
| `persistence.storageClass`                         | Storage class of backing PVC                                                                 | `nil` (uses alpha storage class annotation)             |
| `persistence.accessMode`                           | Use volume as ReadOnly or ReadWrite                                                          | `ReadWriteOnce`                                         |
| `persistence.size`                                 | Size of data volume                                                                          | `8Gi`                                                   |
| `persistence.annotations`                          | Persistent Volume annotations                                                                | `{}`                                                    |
| `persistence.existingClaim`                        | Name of an existing PVC to use (avoids creating one if this is given)                        | `nil`                                                   |
| `livenessProbe.initialDelaySeconds`                | Delay before liveness probe is initiated                                                     | `30`                                                    |
| `livenessProbe.periodSeconds`                      | How often to perform the probe                                                               | `10`                                                    |
| `livenessProbe.timeoutSeconds`                     | When the probe times out                                                                     | `5`                                                     |
| `livenessProbe.successThreshold`                   | Minimum consecutive successes for the probe to be considered successful after having failed. | `1`                                                     |
| `livenessProbe.failureThreshold`                   | Minimum consecutive failures for the probe to be considered failed after having succeeded.   | `6`                                                     |
| `readinessProbe.initialDelaySeconds`               | Delay before readiness probe is initiated                                                    | `5`                                                     |
| `readinessProbe.periodSeconds`                     | How often to perform the probe                                                               | `10`                                                    |
| `readinessProbe.timeoutSeconds`                    | When the probe times out                                                                     | `5`                                                     |
| `readinessProbe.failureThreshold`                  | Minimum consecutive failures for the probe to be considered failed after having succeeded.   | `6`                                                     |
| `readinessProbe.successThreshold`                  | Minimum consecutive successes for the probe to be considered successful after having failed. | `1`                                                     |
| `configmap`                                        | MongoDB configuration file to be used                                                        | `nil`                                                   |
| `metrics.enabled`                                  | Start a side-car prometheus exporter                                                         | `false`                                                 |
| `metrics.image.registry`                           | MongoDB exporter image registry                                                              | `docker.io`                                             |
| `metrics.image.repository`                         | MongoDB exporter image name                                                                  | `forekshub/percona-mongodb-exporter`                    |
| `metrics.image.tag`                                | MongoDB exporter image tag                                                                   | `latest`                                                |
| `metrics.image.pullPolicy`                         | Image pull policy                                                                            | `IfNotPresent`                                          |
| `metrics.image.pullSecrets`                        | Specify docker-registry secret names as an array                                             | `[]` (does not add image pull secrets to deployed pods) |
| `metrics.podAnnotations`                           | Additional annotations for Metrics exporter pod                                              | {}                                                      |
| `metrics.resources`                                | Exporter resource requests/limit                                                             | Memory: `256Mi`, CPU: `100m`                            |
| `metrics.serviceMonitor.enabled`                   | Create ServiceMonitor Resource for scraping metrics using PrometheusOperator                 | `false`                                                 |
| `metrics.serviceMonitor.additionalLabels`          | Used to pass Labels that are required by the Installed Prometheus Operator                   | {}                                                      |
| `metrics.serviceMonitor.relabellings`              | Specify Metric Relabellings to add to the scrape endpoint                                    | `nil`                                                   |
| `metrics.serviceMonitor.alerting.rules`            | Define individual alerting rules as required                                                 | {}                                                      |
| `metrics.serviceMonitor.alerting.additionalLabels` | Used to pass Labels that are required by the Installed Prometheus Operator                   | {}                                                      |

You can provide `values.yaml` file or simple use default values provided by chart itself

## Replication
You can start the MongoDB in replica set mode by setting `replicaSet.enabled` to `true`

## Persistence
The [Bitnami MongoDB](https://github.com/bitnami/bitnami-docker-mongodb) image stores the MongoDB data and configurations at the `/bitnami/mongodb` path of the container.
The chart mounts a [Persistent Volume](http://kubernetes.io/docs/user-guide/persistent-volumes/) at this location. The volume is created using dynamic volume provisioning.

---
For more information about chart used by this module visit [stable/mongodb]

[stable/mongodb]: https://github.com/helm/charts/tree/master/stable/mongodb
[MongoDB]: https://www.mongodb.com/