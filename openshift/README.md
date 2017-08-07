# Project Title

This include intructions to setup the cotd CICD demonstration.

## Setup

### Create Projects

oc new-project pipeline-dev --description="Cat of the Day Development Environment" --display-name="Cat Of The Day - Dev"
oc new-project pipeline-test --description="Cat of the Day Test Environment" --display-name="Cat Of The Day - Test"

oc new-project pipeline-prod --description="Cat of the Day Production Environment" --display-name="Cat Of The Day - Prod"

### Install Jenkins
oc new-app jenkins-ephemeral -n pipeline-dev

(The Jenkins login and password is admin:password)

### Enable Jenkins SA manage resources from project Test and Prod
oc policy add-role-to-user edit system:serviceaccount:pipeline-${GUID}-dev:jenkins -n pipeline-test
oc policy add-role-to-user edit system:serviceaccount:pipeline-${GUID}-dev:jenkins -n pipeline-prod

### Enable the pulling of images from Project Dev to Projects Test and Prod
oc policy add-role-to-group system:image-puller system:serviceaccounts:pipeline-test -n pipeline-dev
oc policy add-role-to-group system:image-puller system:serviceaccounts:pipeline-prod -n pipeline-dev


And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc
