# Configuration Policy Controller Has a High Error Rate

## Description

At the moment, all Service Delivery configuration policies are configured to be evaluated every 2
hours when compliant. With this in mind, this alert fires when at least 20% of policy evaluations in
the last 2 hours and 15 minutes ended in non-user errors such as API failures.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See [Checking the Configuration Policy Controller Logs](../../common/config-policy-controller-logs.md).

### 2. Check the Hosted Cluster Kubernetes API Server

If there are Kubernetes API connection errors, verify that the hosted cluster's Kubernetes API
server is responding.

### 3. Check the Hosted Cluster Kubeconfig Secret

If the hosted cluster is healthy and there are Kubernetes API connection errors, see
[Check the Hosted Cluster Kubeconfig Secret](../../common/check-hosted-cluster-kubeconfig.md).

### 4. Restart the Pod

If the kubeconfig was replaced or the kubeconfig is valid but there are still Kubernetes API
connection errors, restart the Configuration Policy controller pod. See
[Restarting the Configuration Policy Controller](../../common/config-policy-controller-restart.md).
