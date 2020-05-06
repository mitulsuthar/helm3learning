# Helm Guide 

## Helm Installation Guide


## Set Kubectl current-context 

View current context in use
```
kubectl config get-contexts
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

## See Helm ENV Variables
Get helm environment variables. 
```
helm env
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

## Add Helm Chart Repository

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

## Inspect or Show Chart Information 
Use Helm Show or Helm Inspect command to find more information about a chart.

Show chart information only - Shows basic information about Chart like apiVersion, appVersion, dependencies, description, maintaineres, etc.
```
helm show chart stable/mysql
```

Show chart's readme information.
```
helm show readme stable/mysql
```

Show chart's default values. 
```
helm show values stable/mysql
```

Show everything about a chart, i.e. Chart, Readme, Values
```
helm show all stable/mysql
```

## Installing a Chart without customization
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

## List all the releases
List all the releases in the default namespace
```
helm ls
```

See what is installed in all the namespaces
```
helm ls -A
```

See what is installed in `helmtesting` namespace.
```
helm ls --namespace helmtesting
```

## Get extended information about a release

Get the notes provided by the chart of the release
```
helm get notes happy-panda --namespace helmtesting
```

Get the generated manifest file for the release
```
helm get manifest happy-panda --namespace helmtesting
```

Get values used to generate the release 
```
helm get values happy-panda --namespace helmtesting
```

Get hooks associated with the release
```
helm get hooks happy-panda --namespace helmtesting
```


## Uninstalling Helm Releases
Uninstall a release by its name.
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

