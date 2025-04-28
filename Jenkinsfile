
pipeline {
    agent any
	tools {nodejs "mynodejs"}
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Dev') {
            steps {
                git 'https://github.com/awstrainersz/test-demogit'
                echo 'content of my file is'
                sh 'cat file1.txt'
            }
        }
		stage('node build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Save Artifacts') {
            steps {
                archiveArtifacts artifacts: '*.txt', followSymlinks: false
            }
        }
    }
}
