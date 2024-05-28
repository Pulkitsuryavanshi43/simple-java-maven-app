node('master'){
    environment{
        SONARSERVER = 'sonarserver'
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "localhost:8081"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "maven-central"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "nexus"
    }
    stage('Checkout Code'){
        checkout scm
    }
    stage('Build'){
        bat "mvn clean install -Dmaven.test.skip=true"
    }
    stage('Test Cases Execution'){
        bat "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
    }
    stage('SonarQube analysis') {
            bat 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=e1c5c5721e83ca938b0dd0da4cc0127b72a7be8a'
        }
    // stage('Archive Artifacts'){
    //     archiveArtifacts artifacts: 'target/*.war'
    // }

    stage("publish to nexus") {
            // steps {
                script {
                    // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
                    pom = readMavenPom file: "pom.xml";
                    // Find built artifact under target folder
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    // Print some info from the artifact found
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    // Extract the path from the File found
                    artifactPath = filesByGlob[0].path;
                    // Assign to a boolean response verifying If the artifact name exists
                    artifactExists = fileExists artifactPath;

                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";

                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],

                                // Lets upload the pom.xml file for additional information for Transitive dependencies
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );

                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                // }
            }
    }
    
    // stage('Publish to nexus repository manager'){
    //     script{
    //         // pom=readMavenPom file:"pom.xml";
    //         def warFile = "**/*.war"
    //         nexusArtifactUploader(
    //             nexusVersion:'nexus3',
    //             protocol:'http',
    //             nexusUrl:'http://localhost:8081',
    //             groupId:'pom.com.mycompany.app',
    //             version:'1.0-SNAPSHOT',
    //             repository:'maven-central',
    //             credentialsId:'nexus',
    //             artifacts:[
    //                 [artifactId:'pom.my-app',
    //                  classifier:'',
    //                  file:warFile,
    //                  type:pom.packaging],
    //                 [
    //                     artifactId:'pom.my-app',
    //                     classifier:'',
    //                     file:'pom.xml',
    //                     // type:'pom'
    //                 ]
    //             ]
    //         )
    //     }
    // }
    stage('Deployment') {
        def warFile = "**/*.war"
        deploy(adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: "http://localhost:8090/")],
               contextPath: '',
               war: warFile,
              )
    }
}
