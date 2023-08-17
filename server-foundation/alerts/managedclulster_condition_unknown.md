## ManagedClusterConditionUnknown

Get the ManagedCluster resource and check whether the condition `ManagedClusterJoined` exists.

The following command is run against the `hub` cluster:
```bash
oc get managedcluster <managedcluster_name> -o yaml
```

Before we go any futher, you should know that **in next steps commands are run against the management cluster**, please make sure you switch to the correct kubeconfig or context.

The we need to know the `<klusterlet_name>` of the managed cluster:

```bash
oc get klusterlet | grep <managed_cluster_name>
```

### a. If the condition `ManagedClusterJoined` does not exist
If the condition `ManagedClusterJoined` doesn't exist, it means the managed cluster is not imported successfully. Then check the condtion messages of `Klusterlet`. Especially, condition `Available` and `ReadyToApply`.

```bash
oc get klusterlet <klusterlet_name> -o=jsonpath='{.status.conditions[?(@.type=="Available")]}'
```

The `status` of `Available` condition should be `True`, if not, please check the message of it.

```bash
oc get klusterlet <klusterlet_name> -o=jsonpath='{.status.conditions[?(@.type=="ReadyToApply")]}'
```

The `ReadyToApply` condition is not always exist, it's only exist when the klusterlet is not prepared successfully. In that case, the `status` of it should be `false` and the message should contain the reason of the failure.

If the messages of conditions are not enough for troubleshooting, please record `Klusterlet` CR's detail and share it with the dev team.

### b. If the condition `ManagedClusterJoined` exists

In this case, we need to check the condition `HubConnectionDegraded` message of the `Klusterlet`.

```bash
oc get klusterlet <klusterlet_name> -o=jsonpath='{.status.conditions[?(@.type=="HubConnectionDegraded")]}'
```

A common issue is that CA in a kubeconfig is expired. In that case we need to delete the `<managed_cluster_name>-import` secret in the namespace `<managed_cluster_name>` on the hub cluster. That will trigger the hub cluster to propagate the latest CA bundles to the management cluster.

```bash
oc delete klusterlet <klusterlet_name>
```
