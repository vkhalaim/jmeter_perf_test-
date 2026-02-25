pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

    stages {
        stage('Run JMeter Test') {
            steps {
                sh '''
                mkdir -p reports/jmeter

                /opt/jmeter/apache-jmeter-5.6.3/bin/jmeter \
                  -n \
                  -t Products-Test.jmx \
                  -Jusers=3 \
                  -Jramp_up=5 \
                  -Jduration=600 \
                  -l reports/jmeter/results.jtl \
                  -e \
                  -o reports/jmeter/html
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