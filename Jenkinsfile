pipeline {
    agent any

    options {
        disableConcurrentBuilds()
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
                  -Jusers=3 \
                  -Jramp_up=5 \
                  -Jduration=600 \
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