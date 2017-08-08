# Cotd CI/CD Demonstration

This include intructions to setup the cotd CICD demonstration.

## Setup

### Create Projects
```
oc new-project pipeline-dev --description="Cat of the Day Development Environment" --display-name="Cat Of The Day - Dev"
oc new-project pipeline-test --description="Cat of the Day Test Environment" --display-name="Cat Of The Day - Test"
oc new-project pipeline-prod --description="Cat of the Day Production Environment" --display-name="Cat Of The Day - Prod"
```

### Install Jenkins
```
oc new-app jenkins-ephemeral -n pipeline-dev
```
The Jenkins login and password is admin:password


### Enable Jenkins SA manage resources from project Test and Prod
```
oc policy add-role-to-user edit system:serviceaccount:pipeline-dev:jenkins -n pipeline-test
oc policy add-role-to-user edit system:serviceaccount:pipeline-dev:jenkins -n pipeline-prod
```
OR
```
oc policy add-role-to-user edit -z jenkins -n pipeline-test
oc policy add-role-to-user edit -z jenkins -n pipeline-prod
```

### Enable the pulling of images from Project Dev to Projects Test and Prod
```
oc policy add-role-to-group system:image-puller system:serviceaccounts:pipeline-test -n pipeline-dev
oc policy add-role-to-group system:image-puller system:serviceaccounts:pipeline-prod -n pipeline-dev
```
OR
```
oc policy add-role-to-group system:image-puller  pipeline-test -n pipeline-dev
oc policy add-role-to-group system:image-puller  pipeline-prod -n pipeline-dev
```

### Deploy the COTD Application 

1. Deploy in Development:
```
oc new-app https://github.com/azipory/cotd.git -n pipleline-dev
```
2. Prepare images for Test and Production:
```
oc tag cotd:latest cotd:testready -n pipeline-dev
oc tag cotd:testready cotd:prodready -n pipeline-dev
```
Check images have been taged:
```
# oc get is
NAME      DOCKER REPO                             TAGS                         UPDATED
cotd      172.30.179.232:5000/pipeline-dev/cotd   prodready,testready,latest   35 minutes ago
```
3. Deploy cotd app in Test and Production:
```
oc new-app pipeline-dev/cotd:testready --name=cotd -n pipeline-test
oc new-app pipeline-dev/cotd:prodready --name=cotd -n pipeline-prod
```
4. Create routes:
```
oc expose service cotd -n pipeline-dev
oc expose service cotd -n pipeline-test
oc expose service cotd -n pipeline-prod
```
5. Disable automatic deployment:
```
oc get dc cotd -o yaml -n pipeline-dev | sed 's/automatic: true/automatic: false/g' | oc replace -f -
oc get dc cotd -o yaml -n pipeline-test| sed 's/automatic: true/automatic: false/g' | oc replace -f -
oc get dc cotd -o yaml -n pipeline-prod | sed 's/automatic: true/automatic: false/g' | oc replace -f -
```
### Import Build Config for Jenkins

#### Use the OpenShift web console :
a. Log in to your OpenShift web console.
b. Select your dev project and click Add to Project.
c. Select the Import YAML/JSON tab, paste the Build Config pipeline text, from cotd.yaml file,  in the text box and click Create:

![alt text](download.png)

