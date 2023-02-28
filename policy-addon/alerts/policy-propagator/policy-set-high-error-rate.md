# Policy Set Controller Has a High Error Rate

## Description

The Policy Set controller is part of the Policy Propagator that runs on the ACM Hub (Service
Cluster) and is responsible for `PolicySet` (group of policies) status aggregation and recording it
in the `PolicySet` status section.

This alert has fired because the controller's reconcile error rate is very high. If the reconcile
error rate is high, it could be benign, however, more investigation is recommended.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See [Checking the Policy Propagator Logs](../../common/policy-propagator-logs.md) and filter for
logs with `policy-set` in them to look for any clues.

### 2. Restart the Pod

Restarting the pod will cause all `PolicySet` objects to be reconciled which may resolve the issue.
See [Restarting the Policy Propagator Pod](../../common/policy-propagator-restart.md).
