# Deploy whoami app to docker-desktop on windows host with wsl2

[How-to install ingress](https://kubernetes.github.io/ingress-nginx/deploy/)

Install ingress

    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/cloud/deploy.yaml
    kubectl get pods --namespace=ingress-nginx
    kubectl get service ingress-nginx-controller --namespace=ingress-nginx

Add whoami.local to hosts

    127.0.0.1 whoami.local

## I. Deploy using commands

Deploy whoami app

    kubectl create deployment whoami --image=containous/whoami --port=80
    kubectl expose deployment whoami
    kubectl create ingress whoami --class=nginx --rule="whoami.local/*=whoami:80"

Test

    kubectl get pods
    curl http://whoami.local

Scale pods/mnt/c/Users/user/Development/

    kubectl scale --replicas=2 deployment whoami
    kubectl get pods
    curl http://whoami.local

Cleanup

    kubectl delete deployments.apps whoami
    kubectl delete service whoami
    kubectl delete ingress whoami

## II. Deploy using YAML configs

Deploy whoami app

    kubectl apply -f deployment.yml
    kubectl apply -f service.yml
    kubectl apply -f ingress.yml

Test

    kubectl get pods
    curl http://whoami.local

Scale pods

    kubectl scale --replicas=2 deployment whoami
    kubectl get pods
    curl http://whoami.local

Cleanup

    kubectl delete deployments.apps whoami
    kubectl delete service whoami
    kubectl delete ingress whoami
