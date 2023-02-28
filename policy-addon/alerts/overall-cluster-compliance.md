# Overall Cluster Compliance

This alert fires if any hosted cluster averaged one or more non-compliant policies in the past 30
minutes. Since policy compliance should happen very quickly once policies are deployed, this should
not fire unless something went wrong.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### Examine the Noncompliant Policies

On the ACM Hub (Service Cluster), you can get a list of noncompliant `Policy` objects with the
following command:

```bash
kubectl get policy -A -l policy.open-cluster-management.io/cluster-name -o json | jq '.items[] | select(.status.compliant=="NonCompliant") | {"cluster": .metadata.namespace, "policy": .metadata.name}'
```

The output will look like this:

```json
{
  "cluster": "cluster1",
  "policy": "default.policy1"
}
{
  "cluster": "cluster2",
  "policy": "default.policy1"
}
```

From here, you can look at the YAML of each policy such as with the following command:

```bash
kubectl -n cluster1 get policy default.policy1 -o yaml
```

Then examin the status to see why the policy is noncompliant. The status could look like the
following:

```yaml
status:
  compliant: NonCompliant
  details:
    - compliant: NonCompliant
      history:
        - eventName: default.policy1.1744ab367dfe57eb
          lastTimestamp: "2023-02-17T17:00:11Z"
          message: "NonCompliant; violation - pods not found: [dne] in namespace default missing"
      templateMeta:
        creationTimestamp: null
        name: policy-pod
```
