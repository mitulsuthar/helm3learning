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
A Repository is place that stores different charts. For eg, mysql chart, wordpress chart. For each, chart the repository stores more than one version of the chart. 
Helm Hub is the central location where you can search for a chart across many remotely hosted repositories. These repositories are maintained by different organizations. 

Helm search allows you to search against Hub or against local repositories

Search a chart against Helm Hub across many repositories
```
helm search hub wordpress
```

List local repositories. By default, helm doesn't come with any repositories configured. 
```
helm repo list 
```

Adding a repository
```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

After you have added a repository you can also search against repositories that you have added to your local registry.
```
helm search repo wordpress
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

Search for mysql chart in repo
```
helm search repo mysql
```

## Inspect or Show Chart Information 
Use Helm Show or Helm Inspect command to find more information about a chart.

Show chart information only - Shows basic information about Chart like apiVersion, appVersion, dependencies, description, maintainers, etc.
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

## Customizing a chart before installing

Set one value - For wordpress, change replicaCount from 1 to 4 
 ```
 helm install my-wordpress bitnami/wordpress --set replicaCount=4 --namespace helmtesting --dry-run
```

Set multiple values by providing it as an array of values 
 ```
 helm install my-wordpress bitnami/wordpress --set replicaCount=4,wordpressUsername=Adminotron,wordpressBlogName='Helm Learing Blog' --namespace helmtesting --dry-run
 ```

Set multiple values by providing --set flag multiple times 
```
 helm install my-wordpress bitnami/wordpress --set replicaCount=4 --set wordpressUsername=Adminotron,wordpressBlogName='Helm Learing Blog' --namespace helmtesting --dry-run
```

Change multiple values by providing a .yaml file
```
helm install my-wordpress bitnami/wordpress -f values.yaml --namespace helmtesting --dry-run
```

Change multiple values by providing a .yaml file and --set flag
```
helm install my-wordpress bitnami/wordpress -f values.yaml --set replicaCount=3,wordpressBlogName='Override blog name' --namespace helmtesting --dry-run
```

Enforce a string. i.e. In the example below, the myParam value should be explicitly considered as string value.
```
helm install my-wordpress bitnami/wordpress --set-string myParam=112311 --namespace helmtesting --dry-run
```

Provide your own value by providing your own file. For example, you might want to provide your own script file or certificate file as a parameter. 
```
helm install my-wordpress bitnami/wordpress --set-file scriptFile=myScript.sh --namespace helmtesting --dry-run
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

## Upgrading a Helm Release 
Get helm upgrade information
```
helm upgrade --help
```

Install an older version of a release 
```
helm install happy-panda bitnami/wordpress --version 9.2.3 --set wordpressBlogName='Happy Helming' --namespace helmtesting
```

If no version is specified, helm will upgrade the release to the latest version of the chart.
```
helm upgrade happy-panda bitnami/wordpress --namespace helmtesting 
```

Run History Command to see the history of the release.
```
helm history happy-panda --namespace helmtesting
```

What happens if we run the same upgrade command again
```
helm upgrade happy-panda bitnami/wordpress --namespace helmtesting 
```

Run History command again. You will notice that a new revision was created with the same chart.
```
helm history happy-panda --namespace helmtesting
```

Can I upgrade a release to a lower version? 
```
helm upgrade happy-panda bitnami/wordpress --version 9.2.2 --namespace helmtesting
```

Verify by running the history command again.
```
help history happy-panda --namespace helmtesting
```

Set your own values for release, doesn't respect the old values. 
```
helm upgrade happy-panda bitnami/wordpress --set wordpressBlogName='Happy Helming Blog' --namespace helmtesting
```

Reset all the values to the chart's default values.
```
helm upgrade happy-panda bitnami/wordpress --reset-values --namespace helmtesting
```




## Uninstalling Helm Releases
Do a dry-run before uninstalling a release by its name 
```
helm uninstall happy-panda --namespace helmtesting --dry-run
```

Specify `--namespace` and `--keep-history` flags. `--keep-history` allows you to rollback a release.
```
helm uninstall happy-panda --namespace helmtesting --keep-history
```

Show release status by querying it. Post uninstall it will only show if you had `--keep-history` flag.
```
helm status  happy-panda --namespace helmtesting
```

Helm uninstall without executing any hooks 
```
helm uninstall --no-hooks --namespace helmtesting 
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

