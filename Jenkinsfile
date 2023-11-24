pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test -f pom.xml'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonarqube') {
                        sh 'mvn sonar:sonar -Pcoverage'
                    }
                }
            }
        }
        stage("Quality Gate") {
           steps {
              timeout(time: 10, unit: 'MINUTES') {
               waitForQualityGate abortPipeline: true
             }
           }
         }
        stage('Deploy') {
            steps {
            echo "DEPLOYMENT SUCCESSFUL!!  And the crowd goes wild!"
//                 cat './jenkins/scripts/deploy.sh'
            }
        }
    }
}