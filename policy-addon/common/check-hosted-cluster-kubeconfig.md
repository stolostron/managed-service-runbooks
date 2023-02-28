# Check the Hosted Cluster Kubeconfig Secret

If the logs show Kubernetes API connection errors, verify that the kubeconfig that the Configuration
Policy controller uses to connected to hosted cluster is correct. The kubeconfig can be viewed with
the following command on the hosted cluster's associated management cluster:

```bash
kubectl -n klusterlet-<cluster ID> get secrets config-policy-controller-managed-kubeconfig -o=jsonpath='{.data.kubeconfig}' | base64 -d
```

If the kubeconfig is somehow invalid, replace the kubeconfig in the same manner that it was
generated upon cluster creation. This is out of scope for ACM engineering.
