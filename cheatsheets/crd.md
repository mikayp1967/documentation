# CRD CONTAINERD RUNTIME CLI


    ctr --help                                      Help
    ctr <command> --help                            Command help


    ctr ns ls                                       List namespaces
    ctr ns c michelep                               Create new namespace


## NAMESPACED COMMANDS

Most commands seem to be namespaced and pull requires full repo path

    export CONTAINERD_NAMESPACE=XYZ                     Set default namespace

    ctr [-n NAMESPACE] c ls                             List containers (in specific namespace)

    ctr i pull docker.io/library/nginx:latest           Pull image from dockerhub (
    ctr c info 0653ab1a1cb749c7a410530fa1a10918d8f051062a7aab3e21ee4d232ae6f56e         Info (like docker inspect)


    ctr run -rm -t docker.io/library/alpine:latest sh   Run container (needs full path)    

    ctr task ls                                         List containers and state (namespaced)
        TASK                                                                PID      STATUS
        530cb3eb1900be478b8936acc79104335580d18ecc99ac6a36cf2cf3d6f3ca5c    24450    RUNNING
        a3e4dc758ea2917eb6f0c96bafcff9963d5cdd50b250299eaa7d8fcaef9b2710    24701    RUNNING
        51a119c646377624588d0dc946d708e0a32900d7e8a5f51b3b655d1ad2dbda9f    16504    RUNNING
        c778f966ba8b0444113b70d92206831a9efcfd03a3713119b6993a94d5df4e51    22466    RUNNING

## EXEC IN TO CONTAINER

The following command execs into a container, no clue what exec-id is though. Doesn't seem to correspond to a user but needs setting

    ctr task exec --exec-id abc -t e9ee74990607093b695982a50df303fd9f6cc391293e4424ad75e0dffb05106f sh


## REFERENCES

* https://www.vxav.fr/2021-09-13-interacting-with-containerd-runtime-for-kubernetes/
* https://www.sobyte.net/post/2021-09/containerd-usage/

