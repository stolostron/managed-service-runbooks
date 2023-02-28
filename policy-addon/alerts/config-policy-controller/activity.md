# Configuration Policy Controller Has No Activity

## Description

At the moment, all Service Delivery configuration policies are configured to be evaluated every 2
hours when compliant. With this in mind, this alert fires when there wasn't at least 1 policy that
was evaluated in the last 2 hours and 5 minutes (5 minute grace period). This means the
Configuration Policy controller for that hosted cluster is stuck and needs troubleshooting.

## Procedure to Fix

This is not expected to occur, therefore, there is no prescriptive way to fix it.

## Troubleshooting

### 1. Check the Logs

See [Checking the Configuration Policy Controller Logs](../../common/config-policy-controller-logs.md).

### 2. Check the ConfigurationPolicy Objects

If there is activity in the logs but no errors, verify that there are `ConfigurationPolicy` objects
in the hosted cluster's namespace on the associated management cluster with the following command:

```bash
kubectl -n klusterlet-<cluster ID> get configurationpolicy
```

If there are `ConfigurationPolicy` objects, verify that the evaluation interval was not increased to
above 2 hours with the following command. Note that if the `spec.evaluationInterval` is not set, it
defaults to being evaluated roughly every 10 seconds when the Configuraiton Policy controller is not
saturated.

```bash
kubectl -n klusterlet-<cluster ID> get configurationpolicy -ojsonpath='{.items[*].spec.evaluationInterval}'
```

If the `spec.evaluationInterval` field was modified to be above 2 hours, adjust the Prometheus query
that triggered the alert to compensate for that.

### 3. Restart the Pod

This means the Configuration Policy controller is stuck. See
[Restarting the Configuration Policy Controller](../../common/config-policy-controller-restart.md).
