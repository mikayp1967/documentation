# HELM CHEATSHEET


## Misc helm commands

### Searching for charts & repos
	helm search hub wordpress							        Search global repo for charts
    helm repo list                                              List installed repos
    helm repo add nginx-stable https://helm.nginx.com/stable    Add a repo
	helm search repo nginx                                      

### Downloading/Installing charts

    helm install bitnami/nginx --generate-name                  Install nginx, generate a name for the release
        --create-namespace                                      Create required NS if it doesn't exist
        --wait --timeout [<XXs>]                                Wait for 300s (or specified timeout) to verify pods all running ok

    helm upgrade <release> <package/version>                    Upgrade a release
        --install                                               Install it if it doesn't already exist, else upgrade

    helm pull nginx-stable/nginx-ingress                        Download chart
        --untar                                                 Untar it on download

    helm rollback <release name> <revision>                     Rollback release to specified revision

    helm uninstall <release name>                               Uninstall
        --keep-history                                          Keep history after uninstall

	
### Chart Info & Debugging
	
    helm get notes <chart-name>                                 Get help/usage page for a chart
    helm get values <chart-name>                                Get user defined values for a deployed chart
        --all                                                   Get all values for a deployed chart

    helm list --all -A                                          List all deployed charts in all namespaces
    helm get manifest <installed chart>                         Show manifests as they are running (subtle diferent to output from --dry-run)
    helm history <release name>                                 Release history of a chart


### Misc

    helm create <chart name>                                    Create chart template

### Release Statuses

- pening-install                                    Manifests not yet sent to K8s
- deployed                                          Manifests accepted by K8s
- pending-upgrade                                   Upgrade manifests ready but not yet with K8s
- superseded                                        Release has successfuly been upgraded
- pending-rollback                                  Rollback manifests generated but not yet sent to k8s
- uninstalling                                      Current reease being uninstalled
- uninstalled                                       Last uninstalled release
- failed                                            Install failed
