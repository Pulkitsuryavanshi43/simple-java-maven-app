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
    stage('Deployment') {
        def warFile = "**/*.war"
        deploy(adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: "http://localhost:8090/")],
               contextPath: '',
               war: warFile,
              )
    }
}
