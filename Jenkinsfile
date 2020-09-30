#!groovy

@Library('github.com/ayudadigital/jenkins-pipeline-library@v6.0.1') _

// Initialize global config
cfg = jplConfig('kubernetes-learning', 'doc', '', [email: env.CI_NOTIFY_EMAIL_TARGETS])

pipeline {
    agent { label 'docker' }

    stages {
        stage ('Initialize') {
            steps  {
                jplStart(cfg)
            }
        }
        stage ('Bash linter') {
            steps {
                sh "devcontrol run-bash-linter"
            }
        }
        stage ('Make release') {
            when { branch 'release/new' }
            steps  {
                jplMakeRelease(cfg, true)
                deleteDir()
            }
        }
    }

    post {
        always {
            jplPostBuild(cfg)
        }
    }

    options {
        timestamps()
        ansiColor('xterm')
        buildDiscarder(logRotator(artifactNumToKeepStr: '20',artifactDaysToKeepStr: '30'))
        disableConcurrentBuilds()
        timeout(time: 1, unit: 'DAYS')
    }
}
