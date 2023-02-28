# Policy Status Sync Controller Is Stuck

## Description

The Policy Status Sync controller is responsible for syncing status events of `ConfigurationPolicy`
evaluations to the root `Policy` in the hosted cluster namespace on the management cluster and ACM
Hub.

This alert has fired because the controller's reconcile queue has not decreased in the last 15
minutes, which means the controller is stuck.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See
[Checking the Governance Policy Framework Logs](../../common/governance-policy-framework-logs.md)
and filter by log messages that have `policy-status-sync` in it for any clues.

### 2. Restart the Pod

If there is no activity in the logs, restarting the pod may help. See
[Restarting the Governance Policy Framework Pod](../../common/governance-policy-framework-restart.md).
