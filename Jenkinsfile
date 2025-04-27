pipeline {
    agent any
    environment {
      MAVEN_OPTS="--add-opens java.base/java.lang=ALL-UNNAMED"   
    }   

    stages {
        stage('Test') {
            steps {
                sh 'cd SampleWebApp mvn test'
            }
        }
        stage('Build & Complie') {
            steps {
                sh 'cd SampleWebApp && mvn clean package'
            }
        }

       
        stage('Quality Code Scan Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                sh "mvn -f SampleWebApp/pom.xml sonar:sonar"
            }
        }

        }


        
        stage('Deploy to Tomcat Web Server') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcatID', path: '', url: 'http://3.135.186.87:8080/')], contextPath: 'webapp', war: '**/*.war'
            }
        }
    }

}
