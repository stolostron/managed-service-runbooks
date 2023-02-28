# Policy Template Sync Controller Has a High Error Rate

## Description

The Policy Template Sync controller is responsible for syncing `ConfigurationPolicy` manifests
stored in the root `Policy` object to the hosted cluster's namespace on the management cluster.

This alert has fired because the controller's reconcile error rate is very high. If the reconcile
error rate is high, it could be benign, however, more investigation is recommended.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See
[Checking the Governance Policy Framework Logs](../../common/governance-policy-framework-logs.md)
and filter by log messages that have `policy-template-sync` in it for any clues.

### 2. Restart the Pod

Restarting the pod will cause all root (not replicated) `Policy` objects to be reconciled which may resolve the
issue. See
[Restarting the Governance Policy Framework Pod](../../common/governance-policy-framework-restart.md).
