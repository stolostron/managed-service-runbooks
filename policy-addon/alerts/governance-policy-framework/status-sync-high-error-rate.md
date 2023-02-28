# Policy Status Sync Controller Has a High Error Rate

## Description

The Policy Status Sync controller is responsible for syncing status events of `ConfigurationPolicy`
evaluations to the root `Policy` in the hosted cluster namespace on the management cluster and ACM
Hub. This alert has fired because the controller's reconcile error rate is very high.

A common reason for a reconcile error is trying to update the root `Policy` status but the `Policy`
has since been updated by some other process, so the reconcile is retried. If the reconcile error
rate is high, it could be benign, however, more investigation is recommended.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See
[Checking the Governance Policy Framework Logs](../../common/governance-policy-framework-logs.md)
and filter by log messages that have `policy-status-sync` in it for any clues.

### 2. Restart the Pod

Restarting the pod will cause all root (not replicated) `Policy` objects to be reconciled which may
resolve the issue. See
[Restarting the Governance Policy Framework Pod](../../common/governance-policy-framework-restart.md).
