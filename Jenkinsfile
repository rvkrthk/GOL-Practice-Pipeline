pipeline{
    agent {
        lable 'JAVA_8'
    }
    stages{
        stage{
            steps ("GIT")
            branch "master"
            git "https://github.com/rvkrthk/GOL-Practice-Pipeline.git"
        }
        stage{
            steps ("MAVEN BUILD")
            sh "mvn clean package"
        }
        stage ('Server'){
            steps {
               rtServer (
                    id: "JFROG",
                    url: 'http://http://35.184.142.221:8081/artifactory',
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