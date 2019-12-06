# ship-light

In my everyday work I need to depend many applications for Kubernetes, most of which have Helm charts.
However these Helm charts are not perfect and I need to customize them to fit my needs. To do that I use Kustomize tool.
The problem with that approach is that I need to generate some output from Helm and then apply my customizations on top of that.
And this is what this tool does :)

## Why it's "light"

The tool was inspired by Replicated Ship project, which offers more features than mine, like a nice GUI and many more things.
However I prefer to work with simple editor than a fancy GUI, and that's the reason I created ship-light.

## Usage

Create new directory for the application, then create file /ship-light.conf/ inside it with the following contents (the sample uses cluster-autoscaler from stabe repo of Helm):

```
SHIP_REPO=https://github.com/helm/charts.git
SHIP_PATH=stable/cluster-autoscaler
```

Now, invoke /ship-light/ script:

```
ship-light
```

ship-light will:
- clone the repo if it doesn't exist
- create /values.yaml/ file based on default values from helm chart
- create new namespace based on name of the directory with helm chart (can be overriden with SHIP_NAMESPACE setting in ship-light.conf)
- save current commit id for repo in .ship-commit file
- create /kustomization.yaml/ file
- get helm dependencies and render helm chart to /rendered.yaml/ file

After first usage you can customize values inside /values.yaml/ to change settings of helm chart, you can add some patches or additional resources to /kustomization.yaml/ and then invoke ship-light command again to re-render everything.

## Updating chart

ship-light saves commit of helm chart inside .ship-commit file. With current version, if you want to update helm chart to newer version simply delete .ship-commit file and run ship-light again.

## Available settings

|Name|Description|Required|Default value|
|---|---|---|---|
|SHIP_REPO|URL to repo with helm chart|Yes|None|
|SHIP_PATH|Path inside repo to the directory with helm chart|Yes|None|
|SHIP_NAMESPACE|Overrides default namespace created by ship-light|No|basename $SHIP_PATH|
|SHIP_RELEASE_NAME|Release name for helm chart (used by some charts)|No|basename $SHIP_PATH|
|SHIP_REPO_CLONE_DIR|Path to place a repository with clonned helm chart|No|repo|
|SHIP_DEFAULT_BRANCH|Default branch to checkout during initial clone|No|None|
|SHIP_CAPABILITIES_API|Set api-versions if required by chart|No|None|
