pipeline {
	agent any
	//Can be declared at pipeline or stage level and will be available accordingly.

    parameters {
                string(name: 'firstParams', defaultValue: 'John', description: 'A user that triggers the pipeline')
                text(name: 'DEPLOY_TEXT', defaultValue: 'One\nTwo\nThree\n', description: '')
                booleanParam(name: 'DEBUG_BUILD', defaultValue: true, description: '')
                choice(name: 'CHOICES', choices: ['one', 'two', 'three'], description: '') 
                password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'A secret password')
                }
	environment {
	    OUTPUT_PATH = './output/'
	}

    //Defined at both pipeline and Stage level. But both level option are diffirent.
    options {
        timeout(time: 1, unit: 'HOURS') 
    }

	
	stages {
		
	    stage ('Script Directive clone test') {
	        steps {
				checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'Jenkins_shoby', url: 'https://github.com/Mohdshoaibansari/kubernetes_basics.git']]])		
	        }
	    }
		
	    stage ('Input') {
            //Input is defined at stage level and variables are available on stage level only.
		    input {
			message "Press OK to Continue"
			parameters{
			    string(name:'username', defaultValue: 'User', description: 'Username of the user pressing Ok')
			    string(name:'Number',defaultValue:'one')
					}
			    }
			steps{
				sh 'echo "Input Stage"'
				sh 'echo "Username is ${username} and number is ${Number}"'
				}
	    }
		
		
	    stage ('Environment Variable') {
	        steps {
	            sh 'echo ${OUTPUT_PATH}'
	            
	        }
	    }
		stage ('build') {

		    steps {
			script {
			    def var=fileExists 'README.txt'
				echo '${var}'
			    if(var) { 
				    echo 'Present'
					} else{ 
				    echo 'Absent'
					}
			    }

		        sh 'echo first'
			sh 'mkdir new_dir'
			dir('new_dir') {
				sh 'pwd'
						}
		        sh '''
		        echo "Multiline Step" > test.txt
		        ls -lrt
		        '''
                sh '''
                echo "Username is ${username}"
                echo "Number is ${Number}"
                '''
		    }
			
		}

        stage('run-parallel-branches') {
                steps {
                    parallel(
                        firstStep: {
                            echo "Tests on Linux"
                        },
                        secondStep: {
                            echo "Tests on Windows"
                        }
                    )
                }
                }
        stage('Parameter Check') {
                steps {
                    parallel(
                        firstStep: {
                            sh 'echo "${firstParam}"'
                            sh 'echo "${CHOICES}"'
                        },
                        secondStep: {
                            echo "Tests on Windows"
                        }
                    )
                }
                }
	}

    post {
        always {
            echo "Pipeline completed"
	    cleanWs()
        }
    }
}
