pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                withMaven(maven:'maven3'){
                            sh 'echo build'
                            //sh 'mvn -B -DskipTests clean package'
                    }
            }
        }
        stage('Test') {
            steps {
                withMaven(maven:'maven3'){
                        sh 'echo test'
                //sh 'mvn test'
                }
            }
            post {
                always {
                    sh 'echo post'
                    //tapdTestReport frameType: 'JUnit', onlyNewModified: true, reportPath: '*/target/surefire-reports/*.xml'
                }
            }
        }
        stage('analysis') {
            steps {
                withSonarQubeEnv('DevOpsSonarQube') {
                    withMaven(maven:'maven3'){
                        //sh 'mvn sonar:sonar'
                        sh 'sonar-scanner'
                        //sh 'echo analysis'
                    }
                }
            }
        }
        
        stage('Deploy'){
            steps{
                withMaven(maven:'maven3'){
                sh 'echo deploy'
                    }
                //nexusPublisher nexusInstanceId: 'DevOpsNexus', nexusRepositoryId: 'maven-releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'target/my-app-2.0.jar']], mavenCoordinate: [artifactId: 'my-app', groupId: 'william', packaging: 'jar', version: '${BUILD_NUMBER}']]]
            }
        }
    }
}
