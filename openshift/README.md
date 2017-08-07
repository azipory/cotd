
deploy application
oc new-app https://github.com/azipory/cotd.git -n pipeline-dev

Tag Images for Test/Prod
oc tag cotd:latest cotd:testready -n pipeline-dev
oc tag cotd:testready cotd:prodready -n pipeline-dev

#Deploy App in Test/Prod based on their images
#============================================
oc new-app pipeline-dev/cotd:testready --name=cotd -n pipeline-test
oc new-app pipeline-dev/cotd:prodready --name=cotd -n pipeline-prod

#Create Routes
#=============
oc expose service cotd -n pipeline-dev
oc expose service cotd -n pipeline-test
oc expose service cotd -n pipeline-prod

#Disable Automatic Deployment
#===========================
oc get dc cotd -o yaml -n pipeline-dev  | sed 's/automatic: true/automatic: false/g' | oc replace -f -
oc get dc cotd -o yaml -n pipeline-test | sed 's/automatic: true/automatic: false/g' | oc replace -f -
oc get dc cotd -o yaml -n pipeline-prod | sed 's/automatic: true/automatic: false/g' | oc replace -f -
