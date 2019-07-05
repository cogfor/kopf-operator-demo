# Kopf Operator Demo

This demo implements a Kubernetes Operator using the Pythonic Kopf Operator Framework made by Zalando.

## Run on a Kubernetes cluster

### Build Docker

```sh
docker build -t primedio/ephemeralvolume-operator  .
docker push primedio/ephemeralvolume-operator
```

### Create CRD

```sh
kubectl apply -f deploy/crds/primed_v1alpha1_ephemeralvolume_crd.yaml
```

Check if it installed:

```sh
kubectl get crd

NAME                              AGE
ephemeralvolumeclaims.primed.io   25m
```

### Do deployment

This will create the required RBAC roles & role bindings, and deploy the operator in the `default` namespace.

```sh
kubectl apply -f deploy/
```

## Run locally

```sh
kopf run handlers.py --verbose
```

## Create a custom object

```sh
$ kubectl apply -f deploy/crds/primed_v1alpha1_ephemeralvolume_cr.yaml

NAME       AGE
my-claim   10m
```

After a few seconds, you'll see a PVC created using the parameters set in the custom `evc` resource

```sh
$ kubectl get pvc

NAME       STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
my-claim   Bound     pvc-8262bfc5-9f35-11e9-96e7-ce841daa77c4   10G        RWO            standard       12m
```
