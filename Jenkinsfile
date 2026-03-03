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

    stages {
        stage('Run JMeter Test') {
            steps {
                sh '''
                REPORT_DIR=reports/jmeter/${BUILD_NUMBER}

                mkdir -p ${REPORT_DIR}

                /opt/jmeter/apache-jmeter-5.6.3/bin/jmeter \
                  -n \
                  -t Products-Test.jmx \
                  -Jusers=${USERS} \
                  -Jramp_up=${RAMP_UP} \
                  -Jduration=${DURATION} \
                  -l ${REPORT_DIR}/results.jtl \
                  -e \
                  -o ${REPORT_DIR}/html
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'reports/**'
        }
    }
}