# Cluster dashboard panels for HyperShift addon for SD

## [Fleet dashboard SOPs](./fleet-hypershift-addon-sop.md)
## **Cluster dashboard SOPs**
## [Common dashboard SOPs](./hypershift-addon-sop.md)
  * Clusters: Gauge & Time Series Graph
  * HyperShift Operator (Install Failures)
  * Placement score Failures
  * Kubeconfig Copy Failures

---
## Panel value: **No Data, Unknown or N/A**
## Description
The cluster is not returning metrics data.
## Actions
None

---
## Stat panel: **SLI - Addon (28d)**
## Description
* 28day - This is whether we met the service level indicator for the last 28days with HyperShift addon error rates (addon unavailable)
## Actions
* Find the hosting cluster (management cluster) that is experiencing HyperShift addon outages using the `HyperShift Operator panel` dashboard. Any spikes on this time series indicate which hosting cluster (management cluster) is having issues. You can also look at the HyperShift Operator Install Failures time series, where any spikes will indicate which hosting cluster (management cluster) has seen install failures for the HyperShift operator.
* You can check addon health on the Service Cluster
```
oc get ManagedClusterAddons -A | grep hypershift
```
* The addon deployment is found in the `open-cluster-management-agent-addon` namespace on the Management Cluster (Hosting Cluster)

---
## Stat panel: **SLI - Addon (Range)**
## Description
* Range - This is whether we met the service level indicator for the range chosen for the dashboard with HyperShift addon error rates (addon unavailable)
## Actions
* Find the hosting cluster (management cluster) that is experiencing HyperShift addon outages using the `HyperShift Operator panel` dashboard. Any spikes on this time series indicate which hosting cluster (management cluster) is having issues. You can also look at the HyperShift Operator Install Failures time series, where any spikes will indicate which hosting cluster (management cluster) has seen install failures for the HyperShift operator.
* You can check addon health on the Service Cluster
```
oc get ManagedClusterAddons -A | grep hypershift
```
* The addon deployment is found in the `open-cluster-management-agent-addon` namespace on the Management Cluster (Hosting Cluster)

## Stat panel: **Install**
## Description
* Contains the number of install failures
* Shows whether an install of the HyperShift Operator is progressing
## Actions
* If the Failures has a value other then 0, then the HyperShift Operator install is currently failing.  The install is attempted every 2 minutes.
* If Failures is `0` then the last HyperShift Operator install attempt was successful.

---
## Stat panel: **Addon**
## Description
Shows whether the HyperShift Addon is started on the chosen Hosting Cluster (management cluster).
## Actions
If not started, check the following:
* The addon deployment is found in the `open-cluster-management-agent-addon` namespace on the Management Cluster (Hosting Cluster). Make sure the pod is running and review the events and logs
* You can find the pod on the managed cluster in the namespace `open-cluster-management-agent-addon` and the pod name `hypershift-addon-agent-XXXXX-XXXXX`.

---
## Stat panel: **External DNS**
## Description
Describes whether External DNS is installed, disabled, or unknown
## Actions
* Check the pod which can be found on the Hosting Cluster (Management Cluster) in the `hypershift` namespace if it was enabled correctly.

## Stat panel: **HyperShift Operator**
## Description
Shows the latest state of the HyperShift Operator. The value can be `OK` or `Degraded`.
## Actions
* Check the deployment which can be found on the Hosting Cluster (Management Cluster) in the `hypershift` namespace. Check the logs, events and status if the panel shows `Degraded`

---
## Gauge panel: **Addon Availabile**
## Description
Shows the HyperShift Addon status from the foundation framework. This is the last known status. The guage will show True, False or Unknown.  This instant metric is used over a time window to calculate the SLI.
## Actions
Look at the Actions in `SLI - Addon (28d)`


