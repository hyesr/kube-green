install config
kubectl apply -f https://github.com/kube-green/kube-green/releases/latest/download/kube-green.yaml

create deployment
kubectl create ns sleepme
kubectl -n sleepme create deploy echo-servireplica-1 --image=hashicorp/http-echo
kubectl -n sleepme create deploy do-not-sleep --image=hashicorp/http-echo
kubectl -n sleepme create deploy echo-service-replica-4 --image=hashicorp/http-echo --replicas 4

Setup kube-green in application namespace

To setup kube-green, the SleepInfo resource must be created in sleepme namespace.

The desired configuration is:

    echo-service-replica-1 sleep
    all replicas of echo-service-replica-4 sleep
    do-not-sleep pod is unchanged

At the sleep, echo-service-replica-1 will wake up with the previous 1 replica, and echo-service-replica-4 will wake up with 4 replicas as before.
So, after the sleep, we expect 1 pod active and at after the wake up we still expect 6 pods active.

apply a sleepinfo resource to the desired namespace