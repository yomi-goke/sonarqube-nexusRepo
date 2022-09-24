pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                git 'https://github.com/tkibnyusuf/sonarqube-nexusRepo.git'
            }
        }
        
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sonar-server') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to Nexus') {
        steps {
            nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: '', groupId: 'SampleWebApp', nexusUrl: 'http://54.92.239.209:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
    }
      
        }
        
        stage('Deploy to tomcat') {
        steps{
            deploy adapters: [tomcat9(credentialsId: '258ad60a-09a4-4370-a17d-00ec8bb260f0', path: '', url: 'http://18.209.58.16:8080/')], contextPath: 'myapp', war: '**/*.war'
            
        }
            
        }
}
    
}   
    
    
