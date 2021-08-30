pipeline{
    agent {
        label 'JAVA_8'
    }
    stages{
        stage ('GIT'){
            steps {
            branch "master"
            git "https://github.com/rvkrthk/GOL-Practice-Pipeline.git"
            }
        }
        stage ('MAVEN BUILD'){
            steps {
            sh "mvn clean package"
            }
        }
        stage ('Server'){
            steps {
               rtServer (
                    id: "JFROG",
                    url: 'http://34.135.110.69:8081/artifactory',
                    username: 'admin',
                    password: 'Maxima100%',
                    /*bypassProxy: true,*/
                    timeout: 300
                )
            }
        }
        stage('Upload'){
            steps{
                rtUpload (
                serverId:"JFROG" ,
                    spec: '''{
                        "files": [
                            {
                            "pattern": "*.war",
                            "target": "gameoflife"
                            }
                        ]
                    }''',
                )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "JFROG"
                )
            }
        }
        stage ('Download'){
            steps{
                rtDownload (
                    serverId: 'JFROG',
                    spec: '''{
                        "files": [
                            {
                            "pattern": "*.war",
                            "target": "gameoflife"
                            }
                        ]
                    }''',
                )
            }
        }

    }
    post {
        success{
            junit '**/TEST-*.xml'
            echo 'The build has been successful'
        }
        failure{
            echo 'The build has failed'
        }
    }

}