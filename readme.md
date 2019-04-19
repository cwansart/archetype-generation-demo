This project generates Archetypes of the projects defined in the `projects` array in the `Jenkinsfile`. They need to be Maven projects.

To automate it, install Jenkins (e.g. via Docker) and follow the steps:

* create new job
* enter a name an select _Multibranch Pipeline_ as type
* click on _Add source_ in the _Branch Sources_ section
* enter `https://github.com/cwansart/archetype-generation-demo.git` as _Project Repository_
* 

Make sure you have the Maven and JDK set up as tools with the names _jdk8_ and _maven3.6_ as mentioned in the _Jenkinsfile_.
Also install the Jenkins plugin _[pipeline-utility-steps](https://jenkins.io/doc/pipeline/steps/pipeline-utility-steps/)_.
