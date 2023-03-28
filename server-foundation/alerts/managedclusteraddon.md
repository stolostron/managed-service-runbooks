### Work manager keeps unavailable for 10 minutes

Check the status and conditions of the ManifestWork of the work-manager(the name starts with  `addon-work-manager-deploy`).

And also check the resourceStatus section in this ManifestWork. Each resource’s Applied and Available status should be “True”.

If the ManifestWork is fine and managed cluster is in hosted mode, we also need to check the status of the secret external-managed-kubeconfig.