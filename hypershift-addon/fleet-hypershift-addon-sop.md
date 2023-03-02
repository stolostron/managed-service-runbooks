# Fleet dashboard panels for HyperShift addon for SD

## **Fleet dashboard SOPs**
## [Cluster dashboard SOPs](./cluster-hypershift-addon-sop.md)
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
## Stats panel: **SLI - Addon Availability (28d)**
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
## Stats panel: **SLI - Addon Error Budget (min/28d)**
## Description
* min/28day - This is the remaining error budget which is 20 minutes for a 28 day rolling window.
## Actions
* Use the actions in `SLI -Addon Availability (28d) when this is less than 20 minutes

---
## Stats panel: **SLI - Addon Error Budget (28d)**
## Description
* This is the percentage of the 20 minute error budget that is remaining over the past 28 days.
## Actions
* Use the actions in `SLI -Addon Availability (28d) when this is less than 100%

---
## Time series panel: **SLI - HyperShift Addon Availability**
## Description
* This is a time series graph of the `SLI - Addon Availability (28d)` stats panel
## Actions
* Use the actions in `SLI -Addon Availability (28d) when this is less than 100%

---
## Table panel: Total Hosted Clusters by Hosting Cluster
## Description
Table of Hosting clusters (management clusters) showing the total number of Hosted Control Planes on each
## Actions
* The value will turn yellow at 64 Hosted Control Planes
* The value will turn red at 80 Hosted Control Planes
* When all values are yellow, you should provision a new Hosting Cluster (management cluster)

---
## Time Series panel: Total Hosting Clusters (Management Clusters)
## Description
The total number of Hosting Clusters (management clusters) as a time series graph
## Actions
None

---
## Time series panel: **HyperShift Operator Install Failures**
## Description
In the `Fleet` dashboard, use the HyperShift Addon Installs graph to determine if one of the HyperShift Operator installs is not proceeding correctly. The clusters and the number of install failures is shown in mouse over tooltip. Use the cluster name in the `Cluster` dashboard, for the cluster showing the failure count. The failure count resets to zero if there is a success.
## Actions
* Based on the x-axis, if the install failures are no longer occuring and the cluster dashboard panel `HyperShift Operator` is not degraded, the issue has passed.
* You can find the pod on the managed cluster in the namespace `open-cluster-management-agent-addon` and the pod name `hypershift-addon-agent-XXXXX-XXXXX`.
* There is also an install job that is run to enable the HyperShift operator, this is found in the same namespace and will show an error or completed. There may be multiple attempts.
