def projects = [
    'openliberty'
]

def baseDir = pwd()

node {
    stage('Create archetypes') {
        projects.each { project ->
            sh """
                cd ${project}
                mvn archetype:create-from-project
                cd target/generated-sources/archetype
                mvn clean install
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
                                       -DinteractiveMode=false
                cd demo-app-${id}
                mvn clean verify
            """
        }
    }
}
