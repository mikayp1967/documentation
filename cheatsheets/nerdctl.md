# NERDCTL CONTAINERD CLI

## INSTALL

    https://github.com/containerd/nerdctl/releases


    wget https://github.com/containerd/nerdctl/releases/download/v0.15.0/nerdctl-0.15.0-linux-amd64.tar.gz
    tar Cxzvvf /usr/local/bin nerdctl-0.15.0-linux-amd64.tar.gz

## Namespaces

- List Namespaces
  - crt ns ls
  - nerdctl ns ls
- Set default Namespace
  - export CONTAINERD_NAMESPACE=moby                    Set default namespace (moby for docker, k8s.io for kubernetes)    

Most commands are pretty much the same as docker versions


## REFERENCES

    https://medium.com/nttlabs/nerdctl-359311b32d0e
    https://github.com/containerd/nerdctl
