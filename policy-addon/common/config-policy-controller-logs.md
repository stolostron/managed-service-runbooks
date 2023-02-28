# Checking the Configuration Policy Controller Logs

For the affected hosted cluster, check the logs on the associated management cluster with the
following command:

```bash
kubectl -n klusterlet-<cluster ID> logs deployment/configuration-policy-controller -c config-policy-controller
```
