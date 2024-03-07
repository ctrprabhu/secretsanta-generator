pipeline {
    agent any
    tools { jdk 'jdk11'
             maven 'maven3'
          }
    
    environment {
        SCANNER_HOME= tool 'sonar-scanner'
    }      

    stages {
        stage('Git Checkout') {
            steps {
                git changelog: false, poll: false, url: 'https://github.com/ctrprabhu/secretsanta-generator.git'
            }
        }
        
        stage('Compile') {
            steps {
               sh "mvn clean compile"
            }
        }
        
        stage('Sonarqube-Analysis') {
            steps {
               sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.url=http://192.168.122.145:9000/ -Dsonar.login=squ_0c57b5162c85c51570b4e0142581d6ab5259b6c9 -Dsonar.projectName=secret-santa \
               -Dsonar.java.binaries= . \
               -Dsonar.projectkey=secret-santa '''
            }
        }
        
        stage('OWASP SCan') {
            steps {
                    dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'DP'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        
    }
  }
