# Policy Encryption Keys Controller Has a High Error Rate

## Description

The Policy Encryption Keys controller is part of the Policy Propagator that runs on the ACM Hub
(Service Cluster) and is responsible for rotating the encryption keys in the `policy-encryption-key`
`Secret` objects in the hosted cluster namespaces on the ACM Hub. This `Secret` only exists if you
use the Hub policy template encryption such as the `fromSecret` or `protect` Hub policy template
functions.

This alert has fired because the controller's reconcile error rate is very high. This likely means
that encryption keys can't be rotated for some reason. This likely will have no immediate impact but
it should be investigated to ensure proper security procedures are followed by rotating encryption
keys.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See [Checking the Policy Propagator Logs](../../common/policy-propagator-logs.md) and filter for
logs with `policy-encryption-keys` in them to look for any clues.

### 2. Restart the Pod

Restarting the pod will cause all `policy-encryption-key` `Secret` objects to be reconciled which
may resolve the issue. See
[Restarting the Policy Propagator Pod](../../common/policy-propagator-restart.md).
