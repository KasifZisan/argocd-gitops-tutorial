# ArgoCD
ArgoCD is a tool created by Intuit, that is focused on deploying and managing workloads on Kubernetes. Intuit believe configuration/definition/deployment data for apps and environments should be declarative

## ArgoCD Components
- Kubernetes Cluster
- ArgoCD Service/Application
- Git Repository
- Webhooks

## ArgoCD Applicaiton Lifestyle
- The output of CI should be deployable k8s config stored in a git repo
- k8s config defines an ArgoCD Application custom resource definition
- When this config is added to the git repo, ArgoCD receives a webhook request
- ArgoCD synchronizes with the git repo
    - A git pull
    - Compares what should be running with what is running
    - (Re-)Deploys updated applications
- ArgoCD supports pre and post sync hooks
    - Helm chart hooks don't work
    - ArgoCD is essentially doing ```helm template | kubectl -f -```
- Any changes in the k8s config will cause an application to re-deploy
    - Things you want to cause a re-deploy: docker image tag, helm chart version number
    - Things you may not want: dates, timestamps, any random string
- Undeploy by removing the application from the git repo or it can be configured
- The git repo is the source of truth for how things should be deployed
- The ArgoCD UI (or metrics) is the source of trught for any deployment state
- The ArgoCD Application CR can point to another Application CI and so on
- ArgoCD can be configured to access other Kubernetes clusters
    - Application belong to a project, a project can target an external k8s cluster
- Supports custom deployment methods
    - Blue/Green, Canary
    - ArgoRollouts