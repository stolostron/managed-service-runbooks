### Cluster importing is not finished in 10 minutes

Check the message of the relative Klusterlet’s condition – ReadyToApply, Available.

### Managed cluster keeps unreachable for 10 minutes

Check the message of the relative Klusterlet’s condition – HubConnectionDegraded.

A common issue is that CA in a kubeconfig is expired. In that case and we need to reload override that kubeconfig secret.

There are 2 secrets(containing CA) used in the registration process:
* bootstrap-hub-kubeconfig
* hub-kubeconfig-secret

We need to construct a new hubkubeconfig with cluster-admin user. And make sure “server” is accessible for the managed cluster. And then override the “kubeconfig” field of the secrets with the new kubeconfig.

### Kube-apiserver of managed cluster keeps unavailable for 10 minutes

Check the condition “ManagedClusterConditionAvailable” of the ManagedCluster.

Currently, we have only one reason “ManagedClusterKubeAPIServerUnavailable” when the condition is False.

And it’s message will contain the error returned by the managed cluster discovery client.

### Rotation of managed cluster client cert keeps failed in 24 hours

Check the condition “ClusterCertificateRotated” and its message.