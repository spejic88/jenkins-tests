pipeline {
    agent any

    triggers { cron('H/30 * * * *') }

    environment {
       // test = credentials('technical-credentials')
    }
    stages {
        stage('run-api') {
            steps {
                dir("./app/api/") {
                    catchError {
                        sh "echo 1"
                    }
                }
            }
        } 
        stage('start-docker') {
            steps {
                dir("./app/docker") {
                   sh "echo 2"
                   // sh "docker-compose up -d"
                }

                sleep(time:10, unit:"SECONDS")
                //timeout(time: 10, unit: 'SECONDS')   

                // timeout(10) {
                //      waitUntil {
                //          script {
                //             def r = sh script: 'docker exec -it docker_test_container bash -c "echo 1" -O /dev/null', returnStatus: true
                //             return (r == 1 || r == true);
                //         }
                //     }
                // }  

                
            }            
        }  
        stage('run-tests') {
            steps {               

                script {
                    env.test = "python"
                }
                timeout(time: 600, unit: 'SECONDS') {
                    dir("./app/python") {
                        catchError {
                            sh "echo 3"
                           // sh "python3 test.py"
                        }
                    } 
                }
            }            
        }          
    post {
        always {
            dir("./app/docker) {
                sh "echo 4"
                // sh "docker-compose down"
            }
        }
        success {
            echo 'Successful'
        }
        failure {
            echo 'Failed'
        }
        unstable {
            echo 'Unstable'
        }        
    }
}


