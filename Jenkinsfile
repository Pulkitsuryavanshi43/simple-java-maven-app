node('master'){
    environment{
        SONARSERVER = 'sonarserver'
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
    // stage('Deployment'){
    //     deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path:'', url:'http://localhost:9000/')], contextPath:'counterWebApp', war:'target/*.war'
    // }
    stage('Deployment') {
        // def tomcatDir = 'C:\\path\\to\\your\\apache-tomcat\\apache-tomcat-9.0.55'
        def tomcatDir = 'C:\\Program Files\\Apache Software Foundation\\Tomcat 9.0'
        // def warFile = "C:\\Windows\\System32\\config\\systemprofile\\.m2\\repository\\com\\mycompany\\app\\my-app\\1.0-SNAPSHOT\\my-app-1.0-SNAPSHOT.war"
        def warFile = "**/*.war"

        deploy(adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: "http://localhost:8090/")],
               contextPath: '',
               war: warFile,
               // tomcatInstallation: "Tomcat-${env.NODE_NAME}", // Make sure you have a corresponding Tomcat installation with this name in Jenkins configuration
               removeOldWar: true,
               goal: 'deploy'
              )
    }
}

// pipeline {
//     agent any
//     environment{
//         SONARSERVER = 'sonarserver'
//     }
//     stages {
//         stage('SonarQube analysis') {
//             steps {
//                 withSonarQubeEnv("${SONARSERVER}") {
//                     sh 'mvn clean package sonar:sonar'
//                     }
//             }
//         }
//         stage('Build') {
//             steps {
//                 sh 'mvn -B -DskipTests clean package'
//             }
//         }
//         stage('Test') {
//             steps {
//                 sh 'mvn test'
//             }
//             post {
//                 always {
//                     junit 'target/surefire-reports/*.xml'
//                 }
//             }
//         }
//         stage('Deploy to tomcat server') {
//             steps {
//                 script {
//                     sh 'mv ~/.jenkins/workspace/fp-java-maven/target/root.war /usr/local/Cellar/tomcat/10.1.9/libexec/webapps/root.war'
//                 }
//             }
//         }
//     }
// }
