pipeline {
    agent any
    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'ref', value: '$.ref']
            ],

            causeString: 'Triggered on $ref',

            token: 'test',

            printContributedVariables: true,
            printPostContent: true,

            silentResponse: false,

            regexpFilterText: '$ref',
            regexpFilterExpression: 'refs/heads/' + "master"
        )
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
                sh 'mvn test-compile'
            }
        }
        stage('Algorithm') {
            steps {
                step([$class: 'PrioraBuilder', mnCommitInterv: 1000, mxCommitInterv: 100000])
            }
        }
        stage('Test') {
            steps {
                /*sh 'chmod u+x ./exectests'*/
                sh './priora/exectests'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        /*stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }*/
    }
}
