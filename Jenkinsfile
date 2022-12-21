pipeline {
    agent any
    tools {
        git 'Default'
    }
    // triggers {
    //     gitlab(triggerOnPush: true, triggerOnMergeRequest: true, branchFilterType: 'All')
    // }

    stages {        
        stage ('checkout'){
            steps{
                script{
                    deleteDir()
                    checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/LizAsraf/hello-flask.git']]])
                    sh 'git fetch --all --tags'
                } 
            }
        }

        node('builder') {
            stage ('build and unit-test') {           
                steps {
                    script{
                        sh """
                            ls
                            python3 add-build-num.py ${BUILD_NUMBER}
                        """
                    }
                }
            }

            stage ('tag and package') {
                steps {
                script{
                        echo "packaging..."
                        sh "ls"
                        sh "tar -cvzf hello-${BUILD_NUMBER}.tar.gz application.py requirements.txt"
                        sh "ls"
                    }
                }
            }
            stage ('Deploy') {
                steps {
                    script{
                        // sh "scp hello-${BUILD_NUMBER}.tar.gz master"                              
                    }
                }
            }
        }
    }
    post {
        always {
            script{
                sh 'git clean -if'
                cleanWs()
            }
        }
        success {
            script{
                echo "success"
            }
        }         
        failure {
            script{
                echo "1..2.3 failure"
            }
        }
    }   
}

