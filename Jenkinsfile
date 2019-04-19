def projects = [
    'openliberty'
]

node {
    def jdkPath = tool(name: 'jdk8', type: 'jdk')
    def mvnPath = tool(name: 'maven3.6', type: 'maven')
    def env = [
        "PATH+MVN=${jdkPath}/bin:${mvnPath}/bin",
        "M2_HOME=${mvnPath}",
        "JAVA_HOME=${jdkPath}"
    ]

    withEnv(env) {
        def baseDir = pwd()
        def mavenRepo = "${baseDir}/.m2"

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

        stage('Checkout repository') {
            checkout scm
        }

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

            def id = 1;
            projects.each { project ->
                def pom = readMavenPom(file: "${project}/target/generated-sources/archetype/pom.xml")

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

                id++
            }
        }

        stage('Publish archetype to Nexus') {

        }
    }
}
