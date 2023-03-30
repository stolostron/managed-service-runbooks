## ManifestWorkApplyFailed

The following commands run aginst the `hub` cluster.

There are 3 kinds of `Manifestwork` in SD case:
* Manifestworks for a management cluster itself. For example:
    * `managed_cluster_name="hs-mc-h7nia4tig"; manifestwork_name="addon-governance-policy-framework-deploy-0"`.
* Manifestworks deployed on a management cluster but for a hosted cluster.
    * `managed_cluster_name="hs-mc-h7nia4tig"; manifestwork_name="addon-work-manager-deploy-hosting-237be0bpp47k0pata86cbkqbnnmfasr6-0"`.
    (The `237be0bpp47k0pata86cbkqbnnmfasr6` is the name of the hosted cluster).
* Manifestworks deployed on a hosted cluster, this kind of manifestworks are not related with clusters' provisioning and deprovisioning. For example:
    * `managed_cluster_name="237kgevrqn695m2q3j9g5ku39n5jvn4u"; manifestwork="groups"`.

Check the status of ManifestWork:

```bash
oc get manifestwork <manifestwork-name> -n <managed-cluster-name> -o yaml
```

The structure of the `status` section is:

status
* conditions[array]
* resourceStatus
    * manifests[array]
        * conditions[array]
            * resourceMeta:
                * name
                * namespace
                * group
                * kind
                * etc.

First, we need to take a look at the `Applied` condition of the `ManifestWork`, normally the `stauts` of this condition is `True`. If anything wrong, the `status` will be `False` and the `message` will contain the error message.

Second, we can check `resouceMeta` part. Each `manifest` of the array `manifests` is expected to be `Available` and `Applied`.