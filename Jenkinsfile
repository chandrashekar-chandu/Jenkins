pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Code already checked out from Git'
            }
        }

        stage('Compile Java') {
            steps {
                bat '''
                rmdir /s /q build 2>nul
                mkdir build
                javac -d build src\\Hello.java
                jar cfe hello.jar Hello -C build .
                '''
            }
        }

        stage('Prepare Package Directory') {
            steps {
                bat '''
                mkdir package\\usr\\local\\bin
                copy hello.jar package\\usr\\local\\bin\\
                '''
            }
        }

        stage('Build DEB using FPM') {
            steps {
                bat '''
                fpm -s dir -t deb -n hello-java -v 1.0.%BUILD_NUMBER% -C package .
                '''
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: '*.deb'
        }
    }
}