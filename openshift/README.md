Project Title

One Paragraph of project description goes here

Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

Prerequisites

What things you need to install the software and how to install them

Give examples
Installing

A step by step series of examples that tell you have to get a development env running

Say what the step will be

Give the example
And repeat

until finished
End with an example of getting some data out of the system or using it for a little demo

Running the tests

Explain how to run the automated tests for this system

Break down into end to end tests

Explain what these tests test and why

Give an example
And coding style tests

Explain what these tests test and why

Give an example
Deployment

Add additional notes about how to deploy this on a live system

Built With

Dropwizard - The web framework used
Maven - Dependency Management
ROME - Used to generate RSS Feeds
Contributing

Please read CONTRIBUTING.md for details on our code of conduct, and the process for submitting pull requests to us.

Versioning

We use SemVer for versioning. For the versions available, see the tags on this repository.

Authors

Billie Thompson - Initial work - PurpleBooth
See also the list of contributors who participated in this project.

License

This project is licensed under the MIT License - see the LICENSE.md file for details

Acknowledgments

Hat tip to anyone who's code was used
Inspiration
etc

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
