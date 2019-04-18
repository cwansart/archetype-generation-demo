def projects = [
    'openliberty'
]

def baseDir = pwd()
def mavenRepo = "${baseDir}/.m2"

node {
    properties([
        buildDiscarder(
            logRotator(
                artifactDaysToKeepStr: '',
                artifactNumToKeepStr: '',
                daysToKeepStr: '',
                numToKeepStr: '20')),

            pipelineTriggers([
                cron('H 2 * * *')]
            )
    ])

    stage('Create local maven repository') {
        sh "mkdir ${mavenRepo}"
    }

    stage('Create archetypes') {
        projects.each { project ->
            sh """
                cd ${project}
                mvn archetype:create-from-project
                cd target/generated-sources/archetype
                mvn -Dmaven.repo.local=${mavenRepo} clean install
                cd ${baseDir}
            """
        }
    }

    stage('Build projects') {
        def testDir = "${baseDir}/test-builds"
        sh "mkdir ${testDir} && cd ${testDir}"

        project.each { id, project ->
            def pom = readMavenPom("../${project}/target/generated-sources/archetype/pom.xml")

            sh """
                mvn archetype:generate -DgroupId=com.example \
                                       -DartifactId=demo-app-${id} \
                                       -DarchetypeGroupId=${pom.groupId} \
                                       -DarchetypeArtifactId=${pom.artifactId} \
                                       -DarchetypeVersion=${pom.version} \
                                       -DinteractiveMode=false \
                                       -Dmaven.repo.local=${mavenRepo}
                cd demo-app-${id}
                mvn clean verify
                cd ${testDir}
            """
        }
    }

    stage('Publish archetype to Nexus') {

    }
}
