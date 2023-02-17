# Root Policy Status Has a High Error Rate

## Description

The Root Policy Status controller is part of the Policy Propagator, which runs on the ACM Hub
(Service Cluster) and is responsible for summarizing the compliance state of all hosted clusters in
the root `Policy` objects on the ACM Hub.

This alert has fired because the controller's reconcile error rate is very high. A common reason for
a reconcile error is when a `Policy` object is updated but some other process has updated it during
the reconcile. This will trigger the reconcile to be retried. If the reconcile error rate is high,
it could be benign, however, more investigation is recommended.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See [Checking the Policy Propagator Logs](../../common/policy-propagator-logs.md) and look for any
clues.

### 2. Restart the Pod

Restarting the pod will cause all root (not replicated) `Policy` objects to be reconciled which may
resolve the issue. See
[Restarting the Policy Propagator Pod](../../common/policy-propagator-restart.md).
