# Restarting Governance Policy Framework

For the affected hosted cluster, save the logs on the associated management cluster with the
following command:

```bash
kubectl -n klusterlet-<cluster ID> logs deployment/governance-policy-framework -c governance-policy-framework-addon > governance-policy-framework.log
```

On the same management cluster, restart the pod with the following command:

```bash
kubectl -n klusterlet-<cluster ID> rollout restart deployment/governance-policy-framework
```

Send the pod logs to the ACM engineering team.
