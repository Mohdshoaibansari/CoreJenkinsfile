pipeline {
	agent any
	//Can be declared at pipeline or stage level and will be available accordingly.

    parameters {
                string(name: 'firstParam', defaultValue: 'John', description: 'A user that triggers the pipeline')
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
	    stage ('Script Directive Test') {
	        steps {
			script{
				cleanWs()		
			}
	            
	            sh 'echo ${OUTPUT_PATH}'
	            
	        }
	    }
	    stage ('Environment Variable') {
	        steps {
	            sh 'echo ${OUTPUT_PATH}'
	            
	        }
	    }
		stage ('build') {

		    steps {
		        sh 'echo first'    
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
        }
    }
}
