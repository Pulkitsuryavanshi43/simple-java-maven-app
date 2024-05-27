node('master'){
    stage('checkout code'){
        checkout scm
    }
    stage('Build'){
        sh "mvn clean install -Dmaven.test.skip=true"
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
