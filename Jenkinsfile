pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

    parameters {
        string(name: 'USERS', defaultValue: '50', description: 'Number of users')
        string(name: 'RAMP_UP', defaultValue: '50', description: 'Ramp up in seconds')
        string(name: 'DURATION', defaultValue: '300', description: 'Test duration in seconds')
    }

    environment {
        JMETER_HOME = "/opt/jmeter/apache-jmeter-5.6.3"
        JMETER_TEST_FILE = "Products-Test.jmx"
        REPORT_BASE_DIR = "reports/jmeter"
        JMETER_REPO = "https://github.com/vkhalaim/jmeter_perf_test-"
    }

    

    stages {

        stage('Checkout JMeter Tests') {
            steps {
                git branch: 'main', url: "${JMETER_REPO}"
            }
        }

        stage('Run JMeter Test') {
            steps {
                sh '''
                REPORT_DIR=${REPORT_BASE_DIR}/${BUILD_NUMBER}

                mkdir -p ${REPORT_DIR}

                ${JMETER_HOME}/bin/jmeter \
                    -n \
                    -t ${JMETER_TEST_FILE} \
                    -Jusers=${USERS} \
                    -Jramp_up=${RAMP_UP} \
                    -Jduration=${DURATION} \
                    -l ${REPORT_DIR}/results.jtl \
                    -e \
                    -o ${REPORT_DIR}/html
                '''
            }
        }

        stage('Publish HTML Report') {
            steps {
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: "${REPORT_BASE_DIR}/${BUILD_NUMBER}/html",
                    reportFiles: 'index.html',
                    reportName: 'JMeter Performance Report'
                ])
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'reports/**', fingerprint: true
        }
    }
}