# Kubernetes cheat sheet


## Misc

kubectl explain <command> --recursive                   Show yaml structure for res type

kubectl api-resources                                   Show API resources

kubectl get deploy  -o json |\                          Get deployment scaling info for all deployments
        jq ".items[] | {name: .metadata.name} + .spec.strategy.rollingUpdate"


## Metrics

**Install Metrics**
 
kubectl create -f https://raw.githubusercontent.com/pythianarora/total-practice/master/sample-kubernetes-code/metrics-server.yaml

**K8s top**

kubectl top pod

## Contexts and defaults

  kubectl config get-contexts                       List current contexts
  kubectl config set-context \                      Set default namespace
    --current --namespace=jenkins
  


       
