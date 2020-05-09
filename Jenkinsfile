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

                script {
                    sleep(time: 5, unit: 'SECONDS')
                    timeout(time: 10, unit: 'SECONDS') { 
                       //利用sonar webhook功能通知pipeline代码检测结果，未通过质量阈，pipeline将会fail
                       def qg = waitForQualityGate() 
                           if (qg.status != 'OK') {
                               error "未通过Sonarqube的代码质量阈检查，请及时修改！failure: ${qg.status}"
                        }
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
