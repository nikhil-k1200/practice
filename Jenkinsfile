pipeline {
    agent {
	label { label 'agent2'}
	}
    parameters {
        string(name: 'PERSON', defaultValue: '', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['Build', 'Test', 'Deploy'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages {
		stage('install git') {
		agent {
	label { label 'agent2'}
	}
		steps {
			sh '''yum install git -y
				git -version'''
		}
		}
        stage('Example') {
            agent {
                label 'agent2'
            }
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password for ${params.PERSON}: ${params.PASSWORD}"
            }
        }
    }
}
