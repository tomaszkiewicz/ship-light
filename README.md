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
