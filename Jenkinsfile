node('master'){
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
            steps {
                withSonarQubeEnv("${SONARSERVER}") {
                    // bat 'mvn clean package sonar:sonar'
                    bat 'mvn sonar:sonar'
                    // bat 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 '
                    }
            }
        }
    // stage('Archive Artifacts'){
    //     archiveArtifacts artifacts: 'target/*.war'
    // }
    // stage('Deployment'){
    //     deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path:'', url:'http://localhost:9000/')], contextPath:'counterWebApp', war:'target/*.war'
    // }
    stage('Notification'){
        emailtext(
            subject:"Job Completed",
            body:"Jenkins Pipeline Job for Maven Build got completed!!!",
            to:"build-alerts@example.com"
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
