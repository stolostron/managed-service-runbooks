## HyperShift Addon Dashboard & Alerts
##   Standard Operating Procedures

---
## Event: **No Data, Unknown or N/A**
## Description
The cluster is no longer returning telemetry data.
## Actions
None

---
## Panel: **Clusters: HCP, Available & Total**
## Description
* HCP - Is the number of Hosted Control Planes that are available (Metrics provided for clusters and the fleet). API is accessible
* Available - Is Clusters with an Available Hosted Control Plane and Ready state Node Pool
* Total - Is the total number of Hosted Control Plane clusters, no matter their state (HostedCluster resources) 
## Reading the gauges
    * When clusters are provisioning, the `HCP` & `Available` gauge will be lower then the `Total` gauge
    * If `HCP` is less then `Available` or `Available` is less then `Total`, and no provisioning is occuring, there may be a problem with one of your hosted control planes or its node pool(s). Look at the `Fleet` graph for HyperShift Operator and see if one of the ManagedClusters that hosts control planes is degraded
## Actions
To investigate the gauge panel results
* On the Management Cluster, run `oc get hostedcluster -A`
  * This list represents the `Total` gauge
  * Each row of the output has an Available equals true or false. All rows with true represent the `HCP` gauge.
  * The `Available` gauge is a combination of hostedCluster available: true and nodePool: Ready
* It can take up to 10min for a cluster to register on `HCP` gauge (hostedCluster Availabile: true), then `Available` gauge (HostedCluster Available: true & NodePool: Ready)

---
## Panel: **HyperShift Operator (Degraded)**
## Description
This is the HyperShift Operator controller that manages the hosted control planes. When the status shows degraded, check pod or deployment on the Management Cluster. There is also a historical graph to see when it was degraded
## Actions
To investigate the a degraded state
* The deployment and pods can be found in the namespace `hypershift` with the name `operator` (two pods). 

---
## Panel: **HyperShift Addon Status**
## Description
The HyperShift addon agent, running on a ManagedCluster was successfully started
## Actions
* You can find the pod on the managed cluster in the namespace `open-cluster-management-agent-addon` and the pod name `hypershift-addon-agent-XXXXX-XXXXX`.

---
## Panel: **HyperShift Addon Installs**
## Description
In the `Fleet` dashboard, use the HyperShift Addon Installs graph to determine if one of the HyperShift addon installs is not proceeding correctly. Then view the `Cluster` dashboard, that is showing the failure count. The failure count resets to zero if there is a success.
## Actions
* You can find the pod on the managed cluster in the namespace `open-cluster-management-agent-addon` and the pod name `hypershift-addon-agent-XXXXX-XXXXX`.
* There is also an install job that is run to enable the HyperShift operator, this is found in the same namespace and will show an error or completed. There may be multiple attempts.

---
## Panel: **Placement score failures**
## Description
When there is one or more placement score failures, that can mean the number of hosted clusters could not be collected and scored on a given ManagedCluster for use with Placement.
* This value is incremented by the `hypershift-addon-agent-XXXXX-XXXXX` pod running in the `open-cluster-management-agent-addon` namespace.
## Actions
* Navigate to the `hypershift-addon-agent-XXXXX-XXXXX` pod running in the `open-cluster-management-agent-addon` namespace.
* Open the pod Logs, choose the `hypershift-addon-agent` stream, and search for each of the phrases
    ```
    failed to create or update the addOnPlacementScore
    
    failed to update the addOnPlacementScore
    ```
* This will show you one or more failed attempts to calculate Placement and why

---
## Panel: **Kubeconfig Copy Failures**
## Description
When there is one or more attempts to copy the Hosted Control Plane's kubeconfig for registration of the new hosted cluster into
ACM, this count will be incremented until it is successful. 
## Actions
Check the logs in the `hypershift-addon-agent-XXXXX-XXXXX` pod in the `open-cluster-management-agent-addon` namespace. You should see errors related to copying the kubeconfig. This is on the Management Cluster (Hosting Cluster)

---
## Panel: **HyperShift Addon Available (SLI)**
## Description
If the HyperShift Addon is detected by the ACM Hub as unavailable or unknown (5min heartbeat) it will be marked false or unknown.  This will affect the SLI values. This status is derived from the addon agent framework.
## Action
* You can check addon health with
```
oc get ManagedClusterAddons -A | grep hypershift
```
* The addon is found in the `open-cluster-management-agent-addon` namespace on the Management Cluster (Hosting Cluster)

---
## Panel: **SLI (30day & Range)**
## Description
* 30day - This is whether we met the service level objective in the last 30days for HyperShift addon error rates
* Range - This is whether we met the service level objective over the grafana board's user defined time range for HyperShift addon error rates
## Action
See HyperShift Addon Available (above)