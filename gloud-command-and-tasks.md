# Commands and tasks

## Basics

Setup a new account

```bash
$ gcloud init
...
```

### Logging in

```bash
$ gcloud auth login
You are now logged in as...
```


### Finding all projects

```bash
$ gcloud projects list
PROJECT_ID                NAME                 PROJECT_NUMBER
...
```

### Switching to another project or account

```bash
$ gcloud auth list
                          Credentialed Accounts
ACTIVE  ACCOUNT
*       foo@gmail.com
        bar@gmail.com

To set the active account, run:
    $ gcloud config set account `ACCOUNT`

$ gcloud config set account bar@gmail.com
```


```bash
$ gcloud config set project PROJECT_ID
Updated property [core/project].
```


### Creating a service account

```bash
#!/bin/bash

set -eu

gcloud iam service-accounts create ${SA_ACCOUNT_NAME}
gcloud projects add-iam-policy-binding ${PROJECT_ID} --member "serviceAccount:${SA_ACCOUNT_NAME}@${PROJECT_ID}.iam.gserviceaccount.com" --role "${SA_ROLE}"
gcloud iam service-accounts keys create ${SA_ACCOUNT_NAME}-key.json --iam-account ${SA_ACCOUNT_NAME}@${PROJECT_ID}.iam.gserviceaccount.com

echo Key file is in "${SA_ACCOUNT_NAME}-key.json"
```

## Container registry

### Logging in using a service account key

Suppose the key is in the clipboard

```bash
$ pbpaste | docker login -u _json_key --password-stdin https://eu.gcr.io
```

## Cloud build

## Container registry

```bash
$ gcloud beta container images  list --repository eu.gcr.io/PROJECT_ID/services
NAME
eu.gcr.io/...
```

## Kubernetes Engine

```bash
$ gcloud container clusters list
NAME        LOCATION        MASTER_VERSION  MASTER_IP       MACHINE_TYPE   NODE_VERSION   NUM_NODES  STATUS
app      europe-west3-b  1.14.8-gke.33   35.242.194.244  n1-standard-1  1.14.8-gke.33  2          RUNNING
```

### Logging into a cluster

```bash
$ gcloud container clusters get-credentials app --zone europe-north1-a --project grand-object-265910
```


## IAM
gcloud beta asset search-all-iam-policies --query="policy: bla@foo.de"

### Use a service account

- create the service account
- export the key to <a json file>

```bash
$ export GOOGLE_APPLICATION_CREDENTIALS=<a json file>
...
```

## BigTable

Install cbt:https://cloud.google.com/bigtable/docs/cbt-overview

```bash
$ cbt listinstances
```


## Compute / VM

```bash
gcloud compute ssh vm-internal --zone us-central1-c --tunnel-through-iap
```

## Plain Kubernetes

### Recommended tools

- kubectx
- kubens
- stern

### Debug POD

```bash
$ kubectl run --generator=run-pod/v1 tmp-shell --rm -i --tty --image ubuntu -- /bin/bash
$ kubectl run tmp-shell --rm -i --tty --image nicolaka/netshoot -- /bin/bash
```

### Scale a deployment

```bash
$ kubectl scale deployment.v1.apps/<deploy> --replicas=1
```

### Ambassador

```bash
$ k get mapping
$ k get endpoint
$ k get gw -n ambassador
```

## Istio

This grabs logs from Istiod and gives you a nice snapshot of the events happening in the control plane.

```bash
kubectl logs -n istio-system -l app=istiod
```

```bash
 istioctl analyze
 ```

```bash
 istioctl proxy-status
```

```bash
kubectl logs podid -c istio-proxy
```

## Useful links

https://console.cloud.google.com

https://github.com/GoogleCloudPlatform/cloud-builders

https://cloud.google.com/blog/topics/kubernetes-best-practices

https://cloud.google.com/blog/topics/solutions-how-tos

## Arbitrary notes

```
$ export PROJECT_ID=$(gcloud info --format='value(config.project)')

kubectl get service frontend-external | awk 'BEGIN { cnt=0; } { cnt+=1; if (cnt > 1) print $4; }'
34.105.49.182

curl -o /dev/null -s -w "%{http_code}\n" http://$EXTERNAL_IP

custom.googleapis.com/opencensus/grpc.io/client/roundtrip_latency
```


## Terraform and Terragrunt

## Deployment manager

```
gcloud deployment-manager types list
```

```
gcloud deployment-manager deployments create dminfra --config=config.yaml --preview

gcloud deployment-manager deployments update dminfra

```

---
gcloud container clusters get-credentials $my_cluster --zone $my_zone

kubectl cluster-info

kubectl cluster-info dump

kubectl scale --replicas=3 deployment nginx-deployment

gcloud container node-pools create "temp-pool-1" \
--cluster=$my_cluster --zone=$my_zone \
--num-nodes "2" --node-labels=temp=true --preemptible

export REDIS_IP=$(kubectl get services -l app=redis -o json | jq -r '.items[].spec | select(.selector.role=="master")' | jq -r '.clusterIP')

kubectl auth can-i use podsecuritypolicy/gce.privileged

gcloud container operations list

gcloud components update

gcloud config get-value project -q
