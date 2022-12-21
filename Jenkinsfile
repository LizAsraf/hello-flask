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

        stage ('build and unit-test') {           
            steps {
                node('builder') {
                    script{
                        sh """
                            ls
                            python3.10 add-build-num.py ${BUILD_NUMBER}
                        """
                    }
                }
            }
        }

        // stage ('tag and package') {
        //     node('builder') {
        //         steps {
        //             script{
        //                 echo "packaging..."
        //                 sh "ls"
        //                 sh "tar -cvzf hello-${BUILD_NUMBER}.tar.gz application.py requirements.txt"
        //                 sh "ls"
        //             }
        //         }
        //     }
        // }
        // stage ('Deploy') {
        //     node('builder') {
        //         steps {
        //             script{
        //                 // sh "scp hello-${BUILD_NUMBER}.tar.gz master"                              
        //             }
        //         }
        //     }
        // }
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

