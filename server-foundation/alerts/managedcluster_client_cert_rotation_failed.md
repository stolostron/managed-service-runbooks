## ManagedClusterClientCertRotationFailed

On the hub cluster, check the condition `ClusterCertificateRotated` and its message.

```bash
oc get managedcluster <managedcluster-name> -o=jsonpath='{.status.conditions[?(@.type=="ClusterCertificateRotated")]}'
```
