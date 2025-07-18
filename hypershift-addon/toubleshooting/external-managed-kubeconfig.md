# Hosted Cluster Registration with ACM

To manage a hosted cluster using ACM's tools (like policies), it needs to be registered with the ACM hub. This registration happens when the Cluster Service controller creates a ManagedCluster resource. In a managed service environment, this specific ManagedCluster resource triggers the installation of the klusterlet agent and other necessary agents (like policy add-on agents) within the klusterlet-managedClusterName namespace on the management cluster. These agents then connect and interact with the hosted cluster's Kubernetes API server to perform the management tasks delegated by the ACM hub.

The hypershift-addon agent, also running in the management cluster, monitors these hosted clusters. Once a hosted cluster becomes available, this agent generates the external-managed-kubeconfig secret based on the hosted cluster's admin kubeconfig . This secret is stored in the klusterlet namespace, enabling the klusterlet and other add-on agents to begin managing the hosted cluster.

## External-Managed-Kubeconfig Secret Creation Process

### 1. Trigger Conditions

A successful `external-managed-kubeconfig` secret generation requires the following condition:
1. **HostedCluster** must have `Available` status condition
2. **Admin kubeconfig secret** must exist in the HostedCluster namespace
3. **Target klusterlet namespace** must exist (`klusterlet-<managed-cluster-name>`)
4. **API service** must be available (`kube-apiserver` service in the HostedCluster namespace)

The hypershift-addon agent generates or re-generates the `external-managed-kubeconfig` secret for "available" HostedClusters in the following status changes.
- HostedCluster becomes Available: When HostedClusterAvailable condition changes from non-True to True
- Annotations change: Any modification to HostedCluster annotations
- KubeConfig status changes: When Status.KubeConfig field is updated
- KubeadminPassword status changes: When Status.KubeadminPassword field is updated
- Version history changes: When Status.Version.History is modified

Also, when the hypershift-addon agent restarts.


### 2. Source Data

| Component | Location | Description |
|-----------|----------|-------------|
| **Input Secret** | `<hc-namespace>/<hc-name>-admin-kubeconfig` | Original admin kubeconfig |
| **API Service** | `<hc-namespace>-<hc-name>/kube-apiserver` | Internal API server service |
| **Target Namespace** | `klusterlet-<managed-cluster-name>` | Destination for the secret |

### 3. Transformations

| **Aspect** | **Original** | **Modified** |
|------------|-------------|-------------|
| **Server URL** | `https://api.example.com:6443` | `https://kube-apiserver.namespace-clustername.svc.cluster.local:6443` |
| **Location** | HostedCluster namespace | `klusterlet-<managed-cluster-name>` namespace |
| **Secret Name** | `<hc-name>-admin-kubeconfig` | `external-managed-kubeconfig` |

## Error Handling

### Common Error Scenarios

| **Error Condition** | **Handling** | **Metrics Impact** |
|---------------------|--------------|-------------------|
| **Missing klusterlet namespace** | Waits for namespace creation | Not counted as failure |
| **Invalid kubeconfig format** | Logs error and fails | Increments failure counter |
| **Missing API service** | Returns error and retries | Increments failure counter |
| **Kubeconfig parsing failure** | Logs error and fails | Increments failure counter |

