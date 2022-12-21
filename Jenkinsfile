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
                    checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/LizAsraf/hello-flask.git']]]
                    sh 'git fetch --all --tags'
                } 
            }
        }

        stage ('build and unit-test') {           
            steps {
                node('builder') {
                    script{
                        sh """
                            cd ../hello-flask_master
                            python3.10 add-build-num.py ${BUILD_NUMBER}
                        """
                    }
                }
            }
        }

        stage ('tag and package') {
            steps {
                node('builder') {
                    script{
                        echo "packaging..."
                        sh "ls"
                        sh "tar -cvzf hello-${BUILD_NUMBER}.tar.gz application.py requirements.txt"
                        sh "ls"
                    }
                }
            }
        }
        stage ('Deploy') {
            steps {
                node('builder') {
                    script{
                        // sh "scp hello-${BUILD_NUMBER}.tar.gz master"                              
                    }
                }
            }
        }
    }
    // post {
    //     always {
    //         script{
    //             sh 'git clean -if'
    //             cleanWs()
    //         }
    //     }
    //     success {
    //         script{
    //             echo "success"
    //         }
    //     }         
    //     failure {
    //         script{
    //             echo "1..2.3 failure"
    //         }
    //     }
    // }   
}

