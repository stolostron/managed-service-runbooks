# Configuration Policy Controller Is Often Restarted

## Description

The Configuration Policy controller is not supposed to restart and this alert has fired because it
has restarted at least twice in the last 15 minutes.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See [Checking the Configuration Policy Controller Logs](../../common/config-policy-controller-logs.md).

If activity now appears normal, the issue may have resolved itself.

### 2. Check the Hosted Cluster Kubernetes API Server

If there are Kubernetes API connection errors, verify that the hosted cluster's Kubernetes API
server is responding.

### 3. Check the Hosted Cluster Kubeconfig Secret

If the hosted cluster is healthy and there are Kubernetes API connection errors, see
[Check the Hosted Cluster Kubeconfig Secret](../../common/check-hosted-cluster-kubeconfig.md).
