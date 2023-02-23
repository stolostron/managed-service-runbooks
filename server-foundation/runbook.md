# Runbook of server-foundation alerts

## ManagedCluster

### Alert 1: Cluster importing is not finished in 10 minutes (sev-1)

Check the message of the relative Klusterlet’s condition – ReadyToApply, Available.

### Alert 2: Managed cluster keeps unreachable for 10 minutes (sev-1)

Check the message of the relative Klusterlet’s condition – HubConnectionDegraded.

A common issue is that CA in a kubeconfig is expired. In that case and we need to reload override that kubeconfig secret.

There are 2 secrets(containing CA) used in the registration process:
* bootstrap-hub-kubeconfig
* hub-kubeconfig-secret

We need to construct a new hubkubeconfig with cluster-admin user. And make sure “server” is accessible for the managed cluster. And then override the “kubeconfig” field of the secrets with the new kubeconfig.

### Alert 3: Kube-apiserver of managed cluster keeps unavailable for 10 minutes (sev-2)

Check the condition “ManagedClusterConditionAvailable” of the ManagedCluster.

Currently, we have only one reason “ManagedClusterKubeAPIServerUnavailable” when the condition is False.

And it’s message will contain the error returned by the managed cluster discovery client.

### Alert 4: Rotation of managed cluster client cert keeps failed in 24 hours (sev-3)

Check the condition “ClusterCertificateRotated” and its message.

## ManagedClusterAddon

### Alert 1: Work manager keeps unavailable for 10 minutes (sev-2)

Check the status and conditions of the ManifestWork of the work-manager(the name starts with  `addon-work-manager-deploy`).

And also check the resourceStatus section in this ManifestWork. Each resource’s Applied and Available status should be “True”.

If the ManifestWork is fine and managed cluster is in hosted mode, we also need to check the status of the secret external-managed-kubeconfig.

## ManifestWork

### Alert 1: ManifestWork keeps unprocessed for 10 minutes (sev-2)

In this situation, we need to check the Klusterlet conditions: HubConnectionDegraded and WorkDesiredDegraded.

### Alert 2: The apply of ManifestWork keeps failed for 5 minutes (sev-1/sev-2)

Check the message of ManifestWork condition “Applied”.