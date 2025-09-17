pipeline {
agent any
tools{
maven 'my_maven'
}
stages {
stage('Commit') {
steps {
echo 'This is the start of the pipeline'
sh 'mvn clean'
sh 'mvn compile'
sh 'mvn test'
}
}
}
}
