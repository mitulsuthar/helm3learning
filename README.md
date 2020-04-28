# Helm Guide 

## Helm Installation Guide


## Set Kubectl current-context 

View current context in use
```
kubectl get-contexts
```
Use different context - In the command below we use `docker-desktop`

```
kubectl use-context docker-desktop
```
verify current context
```
kubectl config current-context
```

Create kubernetes namespace for testing
```
kubectl create namespace helmtesting
```

## Verify Helm Version

Run the following code
```
helm  
```

```
helm help  
```

Verify Helm Version
```
helm version
```

## Helm Repo vs Hub
Search a chart against Helm Hub
```
helm search hub wordpress
```

Installing a helm chart
```
helm install my-wordpress bitnami/wordpress --dry-run
```
`Error: Failed to download wordpress` - This is because we haven't added bitnami repository 

Adding a repository
```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

## Initialize Helm Chart Repository

Add chart repository
```
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

List charts from repository
```
helm search repo stable
```

Seearch for mysql chart
```
helm search repo mysql
```

Show chart information
```
helm show repo stable/mysql
```

Show chart full information 
```
helm show all stable/mysql
```

Check before you install step aka `--dry-run`. You can use any other name other than happy-panda.
```
helm install stable/mysql happy-panda --dry-run
```

Dry-Run with namespace 
```
helm install stable/mysql happy-panda --dry-run --namespace helmtesting
```

Finally, install the chart into a namespace with default values
```
helm install stable/mysql happy-panda --namespace helmtesting
```
Check the status of the release 
```
helm status happy-panda --namespace helmtesting
```
See what is installed
```
helm ls
```

See what is installed in all the Namespaces
```
helm ls -A
```

See what is installed in `helmtesting` namespace.
```
helm ls --namespace helmtesting
```

Uninstall mysql release.
```
helm uninstall happy-panda
```

Oops. Didn't find the release. Specify `--namespace` and `--keep-history` flags. `--keep-history` allows you to rollback a release.
```
helm uninstall happy-panda --namespace helmtesting --keep-history
```

Show release status by querying it. Post uninstall it will only show if you had `--keep-history` flag.
```
helm status  happy-panda --namespace helmtesting
```



## Creating Charts

Create a new chart
```
helm create mychart
```

Install a new chart

```
helm install clunky-several ./mychart
```

Get manifests
```
helm get manifest clunky-several
```

Uninstall manifest
```
helm uninstall clunky-several 
```

Dry run install 
```
helm install clunky-several ./mychart --dry-run
```

Install into a namespace 
```
helm install clunky-serveral ./mychart --namespace helmtesting
```

