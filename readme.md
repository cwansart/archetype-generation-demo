This project generates Archetypes of the projects defined in the `projects` array in the `Jenkinsfile`. They need to be Maven projects.

To automate it, install Jenkins (e.g. via Docker) and follow the steps:

* create new job
* enter a name an select _Multibranch Pipeline_ as type
* click on _Add source_ in the _Branch Sources_ section
* enter _https://github.com/cwansart/archetype-generation-demo.git_ as _Project Repository_
*  
