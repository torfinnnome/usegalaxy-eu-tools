pipeline {
	environment {
		GALAXY_API_KEY = credentials('GALAXY_API_KEY')
	}
	agent {
		docker {
			image 'python:2.7'
			args '-u root'
		}
	}

	stages {

		stage('Linting') {
			steps {
				sh 'pip install -r requirements.txt'
				sh 'make lint'
			}
		}

		stage('Updated Trusted Repositories') {
			//when {
			//	branch 'master'
			//}

			steps {
				sh 'git reset --hard origin/master'
				sh 'git remote -v show'
				sh 'git checkout master'
				sh 'git branch'
				sh 'git branch -a'

				sh 'pip install -r requirements.txt'
				sh 'make update_trusted'

				sh 'mkdir -p ~/.ssh'
				sh 'ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts'

				sshagent(['usegalaxy.no']) {
					sh 'git push git@github.com:torfinnnome/usegalaxy-eu-tools.git master'
				}
			}
		}

	}
}
