pipeline {
    agent any

    tools {
        maven 'my_maven'
    }

    stages {
        stage('Build') {
            steps {
                echo 'This is the build stage'
                sh 'mvn clean'
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                echo 'This is the testing stage'
                sh 'mvn test'
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'This is the code analysis stage'
                sh '''
                    mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=HelloWorldTest \
                        -Dsonar.projectName="HelloWorldTest" \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.token=squ_d2aca18bb19dcda4522983a60e53e846baad22e4
                '''
            }
        }

        stage('Deploy to Nexus') {
            steps {
                echo 'This is the deploy stage'
                withCredentials([usernamePassword(
                    credentialsId: 'nexus-credentials',
                    usernameVariable: 'NEXUS_USER',
                    passwordVariable: 'NEXUS_PASS'
                )]) {
                    sh '''
                        curl -v -u $NEXUS_USER:$NEXUS_PASS \
                        --upload-file /var/lib/jenkins/workspace/IntegrationTest/target/HelloWorld-0.0.1.jar \
                        http://localhost:8081/repository/maven-releases2/com/example/HelloWorld/0.0.1/HelloWorld-0.0.1.jar
                    '''
                }
            }
        }
    }
}
post {
    success {
      mail to: 'alex.baitann19@gmail.com',
           subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
           body: "Build succeeded.\nSee: ${env.BUILD_URL}"
    }
    failure {
      mail to: 'alex.baitann19@gmail.com',
           subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
           body: "Build failed.\nConsole: ${env.BUILD_URL}console"
    }
  }
