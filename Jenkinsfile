pipeline {
    environment {
        PATH = "/bin:/usr/local/bin:/usr/bin:/bin/bash"
    }
    agent any
    stages {
        stage ('Pre Clean Up'){
            steps {
                lock('PreCleanUp') {
//                     sh "docker stop \$(docker ps -a -q) || true"
//                     sh "docker rm \$(docker ps -a -q) || true"
//                     sh "docker-compose down -v --rmi all --remove-orphans"
//                     sh "docker system prune -f"
//                     sh "docker volume prune -f"
//                     sh "cp env/app/.env.local env/app/.env"
//                     sh "cp env/docker/.env.local env/docker/.env"
//                     sh "docker-compose up -d --build"
                }
            }
        }
        stage ('Tests'){
            steps {
                lock('Tests') {
//                     sh "docker exec \$(docker ps -q --filter='NAME=mpw_app') /bin/bash -c 'conda init bash && source ~/.bashrc && conda activate mpw && pycodestyle --statistics src/'"
//                     sh "docker exec \$(docker ps -q --filter='NAME=mpw_app') /bin/bash -c 'conda init bash && source ~/.bashrc && conda activate mpw &&  coverage run --branch --source=./src -m unittest discover -s ./test/ -p \"test_*.py\" -b'"
//                     step([
//                         $class: 'CloverPublisher',
//                         cloverReportDir: 'build/coverage',
//                         cloverReportFileName: 'coverage.xml',''
//                         healthyTarget: [methodCoverage: 65, conditionalCoverage: 65, statementCoverage: 65],
//                         unhealthyTarget: [methodCoverage: 30, conditionalCoverage: 30, statementCoverage: 30],
//                         failingTarget: [methodCoverage: 0, conditionalCoverage: 0, statementCoverage: 0]
//                     ])
                }
            }
        }
        stage ('Staging'){
            when {
                anyOf {
                    branch "release/*";
                    branch "hotfix/*"
                }
            }
            steps {
//                 build job: 'bi-churn-features-staging-release', parameters: [[$class: 'StringParameterValue',
//                 name: 'src_dir', value: WORKSPACE], [$class: 'StringParameterValue', name: 'tag_name', value: 'v0.0.0']]
            }
        }
        stage ('Release'){
            when {
                tag ""
            }
            steps {
//                 build job: 'bi-churn-features-production-release', parameters: [[$class: 'StringParameterValue',
//                 name: 'src_dir', value: WORKSPACE], [$class: 'StringParameterValue', name: 'tag_name', value: TAG_NAME]]
            }
        }
        stage ('Post Clean up'){
            steps {
                lock('PostCleanup') {
//                     sh "docker-compose down -v --rmi all --remove-orphans"
//                     sh "docker system prune -f"
//                     sh "docker volume prune -f"
                }
            }
        }
    }
    post {
        success {
            script {
                bitbucketStatusNotify(
                    buildState: 'SUCCESSFUL',
                    credentialsId: '1154cae7-fd28-4ca1-ab5e-042eb729bc91'
                )
            }
        }
        failure {
            script {
                bitbucketStatusNotify(
                    buildState: 'FAILED',
                    credentialsId: '1154cae7-fd28-4ca1-ab5e-042eb729bc91'
                )
            }
        }
    }
}