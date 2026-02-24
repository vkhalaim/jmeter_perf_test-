pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/vkhalaim/jmeter_perf_test-'
            }
        }

        stage('Run JMeter') {
            steps {
                sh '''
                mkdir -p reports/jmeter
                /opt/jmeter/apache-jmeter-5.6.3/bin/jmeter \
                  -n \
                  -t Products-Test.jmx \
                  -l reports/jmeter/results.jtl \
                  -e \
                  -o reports/jmeter/html
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'reports/jmeter/**'
        }
    }
}
