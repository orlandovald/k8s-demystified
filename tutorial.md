# Topics

## Pods

```sh
# Create your first pods
kubectl apply -f ./manifests/01_pod.yaml

# Look at status of the pod 
kubectl get pods -o wide

# Tail the logs
kubectl logs -f my-pod

# Curl your pod for inside it
kubectl exec my-pod -- curl --silent http://localhost:9898/echo -X POST -H "Content-Type: application/json" -d '{"msg":"hello"}'

# Open a port from host machine to the pod listening port
kubectl port-forward my-pod 9898:9898

# Now curl from the host machine
curl --silent http://localhost:9898/echo -X POST -H "Content-Type: application/json" -d '{"msg":"hello"}'

# Delete the pod
kubectl delete pod my-pod
```

## Replicasets

```sh
# create the ReplicaSet
kubectl apply -f manifests/02_replicaset.yaml

# scale the replicas
kubectl scale --replicas=5 rs/my-replicaset

# delete a pod to see another be created
kubectl delete pod my-replicaset-<pod-hash>

# delete the Replicaset
kubectl delete replicaset my-replicaset
```

## Deployments

```sh
# create the Deployment
kubectl apply -f ./manifests/03_deployment.yaml

# describe Deployments
kubectl describe deployment

# port forward to one of the deployment's pods
kubectl port-forward deployment/my-deployment 9898:9898

# scale deployment
kubectl scale --replicas=7 deployment/my-deployment
```

## Services

```sh
# create the Deployment
kubectl apply -f ./manifests/04_service.yaml

# look at service
kubectl get service -o service -o service -o wide

# exec into a pod and curl the service
kubectl exec -it deployment/my-deployment -- sh
# inside the pod
$ curl http://my-service:80
```

## Ingress

```sh
# create the Ingress
kubectl apply -f ./manifests/05_ingress.yaml

# get the Ingress
kubectl get ingress my-ingress
# then open in browser http://localhost:8081/
```

## StatefulSet

```sh
# Create the redis namespace
kubectl create namespace _redis

# Create the redis statefulset
kubectl apply -f ./manifests/06_redis.yaml

# Try to add a key to the cache
curl http://localhost:8081/cache/foo -X POST -d "Hello foo"
```

## ConfigMap and Secrets

```sh
# Create configmap
kubectl apply -f ./manifests/07_configmap.yaml

# Create a secret
kubectl create secret generic my-secret --from-literal=secret-token=foobar123

# Update deployment with configmap volume mount
kubectl apply -f ./manifests/08_deployment-config.yaml

# Confirm configmap was mounted
curl http://localhost:8081/configs

# Confirm env var from secret was added
curl http://localhost:8081/env

# Try again adding / retrieving from the cache
curl http://localhost:8081/cache/foo -X POST -d "Hello foo"
curl http://localhost:8081/cache/foo -v

# what happens if we restart redis?
kubectl rollout restart statefulset/redis -n redis
# try to get a key from the cache again
curl http://localhost:8081/cache/foo -v
```

## PersistentVolumes

```sh
# Create a persistentvolume and a persistentvolumeclaim
kubectl apply -f ./manifests/09_persistentvolume.yaml

# delete and re-create the statefulset now with persistentvolume
kubectl delete statefulset redis -n redis
kubectl apply -f ./manifests/10-redis-pvc.yaml

# now you can use the curl commands above to add keys to the cache and they will survive the restart
```

## Jobs and CronJobs

```sh
# Create a Job
kubectl apply -f ./manifests/11_job.yaml

# Create a CronJob
kubectl apply -f ./manifests/12_cronjob.yaml
```
