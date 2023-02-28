# Checking the Policy Propagator Logs

On the ACM Hub (Service Cluster), run the following command:

```bash
kubectl -n redhat-open-cluster-management logs deployment/grc-policy-propagator -c governance-policy-propagator
```

Note that if there is more than 1 replica, you'll have to examine each pod instead of using `deployment/grc-policy-propagator`.
