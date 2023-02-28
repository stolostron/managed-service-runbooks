# Policy Template Sync Controller Is Stuck

## Description

The Policy Template Sync controller is responsible for syncing `ConfigurationPolicy` manifests
stored in the root `Policy` object to the hosted cluster's namespace on the management cluster.

This alert has fired because the controller's reconcile queue has not decreased in the last 15
minutes, which means the controller is stuck.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See
[Checking the Governance Policy Framework Logs](../../common/governance-policy-framework-logs.md)
and filter by log messages that have `policy-template-sync` in it for any clues.

### 2. Restart the Pod

If there is no activity in the logs, restarting the pod may help. See
[Restarting the Governance Policy Framework Pod](../../common/governance-policy-framework-restart.md).
