# ACM Foundation SOPs for SD

There are several critical informations you need to get before you start troubleshooting, and they can be found in alerts' annotation -- `description`.
* managedcluster_name
* manifestwork_name(only needed when handling a manifestwork alert.)

Another useful annotation is `dashboard`, it contains the url of the dashboard which can help you to find more information about the alert.

The you need to use input the `managed_cluster_name` as the dashboard variable `managedcluster` and check the `ManagedCluster Finder` row.

Then you can find serveral information about this managed cluster in that row:
* **ClusterType**: Whether it's a management cluster or a host cluster.
* **HubClusterID**: The hub cluster ID of this managed cluster.
* **HostedClusters**: If it's a management cluster, then in this table will list all hosted clusters on it.
* **ManagedClusterLabels**:
    * if it's a management cluster, we can get ths osd cluster status and addons status of it.
    * **if it's a hosted cluster, we can get the `management_cluster_name` and the `hosted_cluster_namespace` of it.**

## ManagedCluster
- [ManagedClusterConditionUnknown](./alerts/manaegdcluster.md#managedclusterconditionunknown)
- [ManagedClusterKubeAPIServerUnavailable](./alerts/manaegdcluster.md#managedclusterkubeapiserverunavailable)
- [ManagedClusterClientCertRotationFailed](./alerts/manaegdcluster.md#managedclusterclientcertrotationfailed)

## ManifestWork

- [ManifestWorkApplyFailed](./alerts/manifestwork.md#manifestworkapplyfailed)
