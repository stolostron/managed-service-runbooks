# Governance Policy Framework Is Often Restarted

## Description

The Governance Policy Framework contains the controllers responsible for distributing `Policy`
objects from the Hub to the management cluster for the hosted cluster. It also is responsible for
creating the `ConfigurationPolicy` objects defined in the `Policy` on the management cluster for the
hosted cluster. Additionally, it syncs status from `ConfigurationPolicy` objects to the `Policy` on
the management cluster and the Hub.

This alert has fired because the Governance Policy Framework has been restarted more than twice in
the last 15 minutes.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See
[Checking the Governance Policy Framework Logs](../../common/governance-policy-framework-logs.md).
