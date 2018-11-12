# EKS Workshop Helm Chart

This repository contains a Helm chart for the [EKS Workshop](https://eksworkshop.com) demo application.

## Pre-request

You need to setup [Helm](https://github.com/helm/helm) on a workshop Kubernetes cluster.

```sh
# clone this repository
git clone https://github.com/alexei-led/eksdemo-chart

# change working directory
cd eksdemo-chart
```

### Helper tools

[kubeval](https://github.com/garethr/kubeval) - validate Kubernetes configuration files

## Chart Structure

```text
├── README.md
└── eksdemo
    ├── Chart.yaml
    ├── charts
    ├── templates
    │   ├── NOTES.txt
    │   ├── _helpers.tpl
    │   ├── deployment
    │   │   ├── crystal.yaml
    │   │   ├── frontend.yaml
    │   │   └── nodejs.yaml
    │   └── service
    │       ├── crystal.yaml
    │       ├── frontend.yaml
    │       └── nodejs.yaml
    └── values.yaml
```

The demo application consists from 3 deployments and 3 services:

- `templates\deployment` folder contains deployment templates
- `templates\service` folder contains service templates
- `values.yaml` contains default values for all templates

## Validate/Debug Chart

```sh
# lint demo application chart for errors
helm lint eksdemo

# locally render Kubernetes templates and manually review them
helm template eksdemo

# validate Kubernetes templates for errors
helm template eksdemo | kubeval

# install demo application in a dry-run mode (Release Name: workshop)
helm --debug install --name workshop --dry-run eksdemo
```

## Install/Update Demo application with Helm

### First time application install

```sh
helm --debug install --name workshop eksdemo
```

### Subsequent application upgrades

```sh
helm upgrade workshop eksdemo
```

### Inspect application deployment status

```sh
helm status workshop
```

## Other useful Helm commands

### Fetch release history

```text
helm history workshop
```

sample output:

```text
REVISION	UPDATED                 	STATUS    	CHART        	DESCRIPTION     
1       	Sun Nov 11 13:11:04 2018	SUPERSEDED	eksdemo-0.2.0	Install complete
2       	Sun Nov 11 13:19:29 2018	SUPERSEDED	eksdemo-0.2.0	Upgrade complete
3       	Sun Nov 11 13:32:16 2018	DEPLOYED  	eksdemo-0.2.0	Upgrade complete
```

### Rollback a release to the previous revision

```text
helm rollback workshop
```