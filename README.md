# MoonActive HomeTask
> Install Kind Cluster
installation - [kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
```bash
kind create cluster
```

> Install Jenkins via Helm 
```bash
# Get Jenkins helm repo
helm repo add jenkins https://charts.jenkins.io
helm repo update
# Install Jenkins
helm upgrade -i jenkins jenkins/jenkins --namespace jenkins --create-namespace --wait
# Pull Password after it installed
kubectl exec --namespace jenkins -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/chart-admin-password && echo
# Create Docker login to push new Images
kubectl create secret docker-registry docker-credentials \
    --docker-username='<Username>'  \
    --docker-password='<TOKEN>' \
    -n jenkins
```
> Created Custom Jenkins Agent with Helm 
```bash
docker build -t <registry>/custom-jenkins-agent-v1 . # Docker file on this Repo
docker push <registry>/custom-jenkins-agent-v1 
```

> Access Jenkins with username 'admin' and password from top
```bash
kubectl --namespace jenkins port-forward svc/jenkins 8080:8080 
```

> Lets Create first job type "Pipeline"
```
https://github.com/AlexeiBarabash/ma-dog-facts-service
https://github.com/AlexeiBarabash/ma-time-service
https://github.com/AlexeiBarabash/ma-dog-facts-source
```
```update only the pipeline location which is git```


![jenkinspipeline](screenshots/jenkins-pipeline.png?raw=true "pipeline")

```Repeat the Pipeline step for each service```


![jenkinsview](screenshots/jenkins-view.png?raw=true "Login")

> Deploy the services to the cluster by triggering each build 
![jenkinsbuild](screenshots/jenkins-ma-service.png?raw=true "build")

```After the Servies started we can test them```
>> Time Service
```bash
kubectl port-forward svc/ma-time-service-ma-services 3000:3000 -n default
```
![ma-time](screenshots/ma-time.png?raw=true "ma-time")

>> Random Fact Service
```bash
kubectl port-forward svc/ma-dog-facts-service-ma-services 3001:3000 -n default
```
![ma-facts](screenshots/ma-facts.png?raw=true "ma-facts")

>> Cluster should look like this 


![kpa](screenshots/kpa.png?raw=true "kpa")