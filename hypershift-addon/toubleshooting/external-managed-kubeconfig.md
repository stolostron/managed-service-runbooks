# Hosted Cluster Registration with ACM

To manage a hosted cluster using ACM's tools (like policies), it needs to be registered with the ACM hub. This registration happens when the Cluster Service controller creates a ManagedCluster resource. In a managed service environment, this specific ManagedCluster resource triggers the installation of the klusterlet agent and other necessary agents (like policy add-on agents) within the klusterlet-managedClusterName namespace on the management cluster. These agents then connect and interact with the hosted cluster's Kubernetes API server to perform the management tasks delegated by the ACM hub.

The hypershift-addon agent, also running in the management cluster, monitors these hosted clusters. Once a hosted cluster becomes available, this agent generates the external-managed-kubeconfig secret based on the hosted cluster's admin kubeconfig . This secret is stored in the klusterlet namespace, enabling the klusterlet and other add-on agents to begin managing the hosted cluster.

## External-Managed-Kubeconfig Secret Creation Process

### 1. Trigger Conditions

A successful `external-managed-kubeconfig` secret generation requires the following condition:
1. **HostedCluster** must have `Available` status condition
2. **Admin kubeconfig secret** must exist in the HostedCluster namespace
3. **Target klusterlet namespace** must exist (`klusterlet-<managed-cluster-name>`)
4. **API service** must be available (`kube-apiserver` service in the HostedCluster namespace)

The hypershift-addon agent generates or re-generates the `external-managed-kubeconfig` secret for "available" HostedClusters in the following status changes.
- HostedCluster becomes Available: When HostedClusterAvailable condition changes from non-True to True
- Annotations change: Any modification to HostedCluster annotations
- KubeConfig status changes: When Status.KubeConfig field is updated
- KubeadminPassword status changes: When Status.KubeadminPassword field is updated
- Version history changes: When Status.Version.History is modified

Also, when the hypershift-addon agent restarts.


### 2. Source Data

| Component | Location | Description |
|-----------|----------|-------------|
| **Input Secret** | `<hc-namespace>/<hc-name>-admin-kubeconfig` | Original admin kubeconfig |
| **API Service** | `<hc-namespace>-<hc-name>/kube-apiserver` | Internal API server service |
| **Target Namespace** | `klusterlet-<managed-cluster-name>` | Destination for the secret |

### 3. Transformations

| **Aspect** | **Original** | **Modified** |
|------------|-------------|-------------|
| **Server URL** | `https://api.example.com:6443` | `https://kube-apiserver.namespace-clustername.svc.cluster.local:6443` |
| **Location** | HostedCluster namespace | `klusterlet-<managed-cluster-name>` namespace |
| **Secret Name** | `<hc-name>-admin-kubeconfig` | `external-managed-kubeconfig` |

## Error Handling

### Common Error Scenarios

| **Error Condition** | **Handling** | **Metrics Impact** |
|---------------------|--------------|-------------------|
| **Missing klusterlet namespace** | Waits for namespace creation | Not counted as failure |
| **Invalid kubeconfig format** | Logs error and fails | Increments failure counter |
| **Missing API service** | Returns error and retries | Increments failure counter |
| **Kubeconfig parsing failure** | Logs error and fails | Increments failure counter |

## Flow

<img width="1018" height="1179" alt="image" src="https://github.com/user-attachments/assets/a4168d61-db2b-467f-80b1-38421b79c171" />

## Service Level Indicators (SLIs)

### Overview

SLIs define measurable metrics to monitor the health, performance, and reliability of the external-managed-kubeconfig generation process. These indicators help establish SLOs (Service Level Objectives) and track system behavior.

### 1. Availability SLIs

#### **Secret Generation Success Rate**
- **Definition**: Percentage of successful external-managed-kubeconfig secret generations
- **Metric**: `(kubeconfig_secret_copy_total - kubeconfig_secret_copy_failures) / kubeconfig_secret_copy_total * 100`
- **Target**: ≥ 99.5%
- **Measurement Window**: 5 minutes
- **Purpose**: Measures the reliability of the secret generation process

```promql
# Success rate over 5 minutes
(
  rate(hypershift_addon_agent_kubeconfig_secret_copy_total[5m]) - 
  rate(hypershift_addon_agent_kubeconfig_secret_copy_failures[5m])
) / rate(hypershift_addon_agent_kubeconfig_secret_copy_total[5m]) * 100
```

### 3. Error Rate SLIs

#### **Secret Generation Error Rate**
- **Definition**: Percentage of failed external-managed-kubeconfig generation attempts
- **Metric**: `kubeconfig_secret_copy_failures / kubeconfig_secret_copy_total * 100`
- **Target**: ≤ 0.5%
- **Measurement Window**: 5 minutes
- **Purpose**: Tracks generation failures for reliability monitoring

```promql
# Error rate over 5 minutes
rate(hypershift_addon_agent_kubeconfig_secret_copy_failures[5m]) / 
rate(hypershift_addon_agent_kubeconfig_secret_copy_total[5m]) * 100
```

#### Grafana Dashboard Queries

```promql
# Secret Generation Success Rate
(
  rate(hypershift_addon_agent_kubeconfig_secret_copy_total[5m]) - 
  rate(hypershift_addon_agent_kubeconfig_secret_copy_failures[5m])
) / rate(hypershift_addon_agent_kubeconfig_secret_copy_total[5m]) * 100

# Error Rate Trend
rate(hypershift_addon_agent_kubeconfig_secret_copy_failures[5m])
```

### Alert Thresholds

#### Critical Alerts
- **Secret Generation Success Rate** < 95% for 5 minutes
- **Error Rate** > 5% for 3 minutes

#### Warning Alerts
- **Secret Generation Success Rate** < 99% for 10 minutes
- **Error Rate** > 1% for 5 minutes

### SLI Monitoring Dashboard

```json
{
  "dashboard": {
    "title": "External-Managed-Kubeconfig Generation SLIs",
    "panels": [
      {
        "title": "Success Rate",
        "type": "stat",
        "targets": [
          {
            "expr": "rate(hypershift_addon_agent_kubeconfig_secret_copy_total[5m]) - rate(hypershift_addon_agent_kubeconfig_secret_copy_failures[5m]) / rate(hypershift_addon_agent_kubeconfig_secret_copy_total[5m]) * 100"
          }
        ]
      }
    ]
  }
}
```

## Service Level Objectives (SLOs)

### Overview

SLOs define the target performance levels for the external-managed-kubeconfig generation service. These objectives are based on the SLIs defined above and establish measurable goals for service reliability, performance, and quality.

### SLO Categories

#### **1. Availability SLOs**

##### **Primary SLO: Secret Generation Availability**
- **Objective**: 99.5% of external-managed-kubeconfig secrets are successfully generated
- **Measurement Period**: 30-day rolling window
- **Error Budget**: 0.5% (approximately 3.6 hours of unavailability per month)
- **Measurement Method**: `(successful_generations / total_generation_attempts) * 100`
- **Reporting Frequency**: Daily
- **Escalation Trigger**: < 99.0% for 24 hours

```promql
# 30-day availability calculation
(
  sum(increase(hypershift_addon_agent_kubeconfig_secret_copy_total[30d])) -
  sum(increase(hypershift_addon_agent_kubeconfig_secret_copy_failures[30d]))
) / sum(increase(hypershift_addon_agent_kubeconfig_secret_copy_total[30d])) * 100
```

### SLO Implementation

#### **Error Budget Policy**

| **SLO Category** | **Error Budget** | **Burn Rate Alert** | **Action Required** |
|------------------|------------------|---------------------|---------------------|
| **Availability** | 0.5% (3.6h/month) | 25% in 1 hour | Immediate investigation |

#### **SLO Monitoring Dashboard**

```json
{
  "dashboard": {
    "title": "External-Managed-Kubeconfig SLOs",
    "time": {
      "from": "now-30d",
      "to": "now"
    },
    "panels": [
      {
        "title": "Availability SLO (99.5%)",
        "type": "stat",
        "targets": [
          {
            "expr": "(sum(increase(hypershift_addon_agent_kubeconfig_secret_copy_total[30d])) - sum(increase(hypershift_addon_agent_kubeconfig_secret_copy_failures[30d]))) / sum(increase(hypershift_addon_agent_kubeconfig_secret_copy_total[30d])) * 100",
            "legendFormat": "30-day Availability"
          }
        ],
        "fieldConfig": {
          "defaults": {
            "thresholds": {
              "steps": [
                {"color": "red", "value": 0},
                {"color": "yellow", "value": 99.0},
                {"color": "green", "value": 99.5}
              ]
            }
          }
        }
      },
      {
        "title": "Error Budget Burn Rate",
        "type": "graph",
        "targets": [
          {
            "expr": "rate(hypershift_addon_agent_kubeconfig_secret_copy_failures[1h]) / rate(hypershift_addon_agent_kubeconfig_secret_copy_total[1h]) * 100",
            "legendFormat": "Hourly Error Rate"
          }
        ]
      }
    ]
  }
}
```

#### **Alerting Rules**

```yaml
# SLO Alerting Rules
groups:
  - name: external-managed-kubeconfig-slo
    rules:
      # Availability SLO
      - alert: KubeconfigGenerationAvailabilitySLOBreach
        expr: |
          (
            sum(increase(hypershift_addon_agent_kubeconfig_secret_copy_total[30d])) -
            sum(increase(hypershift_addon_agent_kubeconfig_secret_copy_failures[30d]))
          ) / sum(increase(hypershift_addon_agent_kubeconfig_secret_copy_total[30d])) * 100 < 99.5
        for: 1h
        labels:
          severity: critical
          slo: availability
        annotations:
          summary: "External-managed-kubeconfig generation availability SLO breached"
          description: "30-day availability is {{ $value }}%, below 99.5% SLO target"
      
      # Error Budget Burn Rate
      - alert: KubeconfigGenerationErrorBudgetBurnRate
        expr: |
          rate(hypershift_addon_agent_kubeconfig_secret_copy_failures[1h]) / 
          rate(hypershift_addon_agent_kubeconfig_secret_copy_total[1h]) * 100 > 12.5
        for: 5m
        labels:
          severity: critical
          slo: error-budget
        annotations:
          summary: "External-managed-kubeconfig generation error budget burning too fast"
          description: "Error rate is {{ $value }}%, will exhaust monthly budget in 4 hours"
```