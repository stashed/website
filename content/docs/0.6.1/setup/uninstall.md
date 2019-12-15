---
title: Uninstall
description: Stash Uninstall
menu:
  docs_0.6.1:
    identifier: uninstall-stash
    name: Uninstall
    parent: setup
    weight: 20
product_name: stash
menu_name: docs_0.6.1
section_menu_id: setup
info:
  version: 0.6.1
---

# Uninstall Stash
Please follow the steps below to uninstall Stash:

1. Delete the deployment and service used for Stash operator.
```console
$ ./hack/deploy/uninstall.sh
+ kubectl delete deployment -l app=stash -n kube-system
deployment "stash-operator" deleted
+ kubectl delete service -l app=stash -n kube-system
service "stash-operator" deleted
+ kubectl delete secret -l app=stash -n kube-system
No resources found
+ kubectl delete serviceaccount -l app=stash -n kube-system
No resources found
+ kubectl delete clusterrolebindings -l app=stash -n kube-system
No resources found
+ kubectl delete clusterrole -l app=stash -n kube-system
No resources found
+ kubectl delete initializerconfiguration -l app=stash
initializerconfiguration "stash-initializer" deleted
```

2. Now, wait several seconds for Stash to stop running. To confirm that Stash operator pod(s) have stopped running, run:
```console
$ kubectl get pods --all-namespaces -l app=stash
```

3. To keep a copy of your existing `Restic` objects, run:
```console
kubectl get restic.stash.appscode.com --all-namespaces -o yaml > data.yaml
```

4. To delete existing `Restic` objects from all namespaces, run the following command in each namespace one by one.
```
kubectl delete restic.stash.appscode.com --all --cascade=false
```

5. Delete the old CRD-registration.
```console
kubectl delete crd -l app=stash
```
