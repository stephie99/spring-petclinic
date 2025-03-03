pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh './mvnw clean package'
                    } else {
                        bat 'mvnw.cmd clean package'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh './mvnw test'
                    } else {
                        bat 'mvnw.cmd test'
                    }
                }
            }
        }

        stage('Jacoco Report') {
            steps {
                jacoco(
                    execPattern: '**/target/*.exec',
                    classPattern: '**/classes',
                    sourcePattern: '**/src/main/java',
                    inclusionPattern: '**/*.class',
                    exclusionPattern: '**/*Test.class'
                )
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            junit '**/target/surefire-reports/*.xml'
            jacoco()
        }
    }
}
