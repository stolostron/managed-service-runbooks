# Restarting the Configuration Policy Controller

For the affected hosted cluster, save the logs on the associated management cluster with the
following command:

```bash
kubectl -n klusterlet-<cluster ID> logs deployment/configuration-policy-controller -c config-policy-controller > config-policy-controller.log
```

On the same management cluster, restart the pod with the following command:

```bash
kubectl -n klusterlet-<cluster ID> rollout restart deployment/configuration-policy-controller
```

Send the pod logs to the ACM engineering team.
