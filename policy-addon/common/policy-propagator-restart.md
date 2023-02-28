# Restarting the Policy Propagator

On the ACM Hub (Service Cluster), save the logs with the following command:

```bash
kubectl -n redhat-open-cluster-management logs deployment/grc-policy-propagator -c governance-policy-propagator > grc-policy-propagator.log
```

Restart the pod with the following command:

```bash
kubectl -n redhat-open-cluster-management rollout restart deployment/grc-policy-propagator
```

Send the pod logs to the ACM engineering team.
