# Checking the Governance Policy Framework Logs

The Governance Policy Framework pod runs several controller such as:

- policy-spec-sync
- policy-status-sync
- policy-template-sync

For the affected hosted cluster, check the logs on the associated management cluster with the
following command:

```bash
kubectl -n klusterlet-<cluster ID> logs deployment/governance-policy-framework -c governance-policy-framework-addon
```
