## Create and run a simple task

### Commands

```
oc project pipelines-demo

oc apply -f tasks/task.yaml

tkn task start --showlog hello
```


## Create and run a pipeline

### Pipeline Diagram
![Pipeline Diagram](images/pipeline-diagram.png)


### Commands
```
oc create -f tasks/apply_manifest_task.yaml
oc create -f tasks/update_deployment_task.yaml

tkn task ls

oc create -f resources/persistent_volume_claim.yaml

oc create -f pipeline/pipeline.yaml

tkn pipeline ls
```

### Run pipeline to build and deploy app backend

```
tkn pipeline start build-and-deploy \
    -w name=shared-workspace,claimName=source-pvc \
    -p deployment-name=pipelines-vote-api \
    -p git-url=https://github.com/openshift/pipelines-vote-api.git \
    -p IMAGE=image-registry.openshift-image-registry.svc:5000/pipelines-demo/vote-api \
    --showlog

```

### Run pipeline to build and deploy frontend

```
tkn pipeline start build-and-deploy \
    -w name=shared-workspace,claimName=source-pvc \
    -p deployment-name=pipelines-vote-ui \
    -p git-url=https://github.com/openshift/pipelines-vote-ui.git \
    -p IMAGE=image-registry.openshift-image-registry.svc:5000/pipelines-demo/vote-ui \
    --showlog
```

```
tkn pipelinerun ls
```


### Get application route

```
oc get route pipelines-vote-ui --template='http://{{.spec.host}}
```