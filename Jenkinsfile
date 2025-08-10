pipeline {
    agent any

    parameters {
        // Параметры для UTG-1
        string(name: 'TG1_START_THREADS', defaultValue: '300', description: 'UTG-1: Number of starting threads')
        string(name: 'TG1_INITIAL_DELAY', defaultValue: '0', description: 'UTG-1: Initial delay (seconds)')
        string(name: 'TG1_STARTUP_TIME', defaultValue: '720', description: 'UTG-1: Startup time (seconds)')
        string(name: 'TG1_HOLD_TIME', defaultValue: '14400', description: 'UTG-1: Hold time (seconds)')
        string(name: 'TG1_SHUTDOWN_TIME', defaultValue: '0', description: 'UTG-1: Shutdown time (seconds)')
        string(name: 'TG1_THROUGHPUT', defaultValue: '9.6', description: 'UTG-1: Target throughput (requests/min)')

        // Параметры для UTG-2
        string(name: 'TG2_START_THREADS', defaultValue: '147', description: 'UTG-2: Number of starting threads')
        string(name: 'TG2_INITIAL_DELAY', defaultValue: '0', description: 'UTG-2: Initial delay (seconds)')
        string(name: 'TG2_STARTUP_TIME', defaultValue: '720', description: 'UTG-2: Startup time (seconds)')
        string(name: 'TG2_HOLD_TIME', defaultValue: '14400', description: 'UTG-2: Hold time (seconds)')
        string(name: 'TG2_SHUTDOWN_TIME', defaultValue: '0', description: 'UTG-2: Shutdown time (seconds)')
        string(name: 'TG2_THROUGHPUT', defaultValue: '8.5714', description: 'UTG-2: Target throughput (requests/min)')

        // Параметры для UTG-3
        string(name: 'TG3_START_THREADS', defaultValue: '71', description: 'UTG-3: Number of starting threads')
        string(name: 'TG3_INITIAL_DELAY', defaultValue: '0', description: 'UTG-3: Initial delay (seconds)')
        string(name: 'TG3_STARTUP_TIME', defaultValue: '720', description: 'UTG-3: Startup time (seconds)')
        string(name: 'TG3_HOLD_TIME', defaultValue: '14400', description: 'UTG-3: Hold time (seconds)')
        string(name: 'TG3_SHUTDOWN_TIME', defaultValue: '0', description: 'UTG-3: Shutdown time (seconds)')
        string(name: 'TG3_THROUGHPUT', defaultValue: '9.2958', description: 'UTG-3: Target throughput (requests/min)')
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/NekiyTot/firstReposit.git', branch: 'main'
            }
        }

        stage('Run JMeter Test') {
            steps {
                script {
                    def jmeterCommand = """
                    "${env.JMETER_HOME}/bin/jmeter" \
                    -n \
                    -t "search_max_test_template.jmx" \
                    -JTG1_START_THREADS=${params.TG1_START_THREADS} \
                    -JTG1_INITIAL_DELAY=${params.TG1_INITIAL_DELAY} \
                    -JTG1_STARTUP_TIME=${params.TG1_STARTUP_TIME} \
                    -JTG1_HOLD_TIME=${params.TG1_HOLD_TIME} \
                    -JTG1_SHUTDOWN_TIME=${params.TG1_SHUTDOWN_TIME} \
                    -JTG1_THROUGHPUT=${params.TG1_THROUGHPUT} \
                    -JTG2_START_THREADS=${params.TG2_START_THREADS} \
                    -JTG2_INITIAL_DELAY=${params.TG2_INITIAL_DELAY} \
                    -JTG2_STARTUP_TIME=${params.TG2_STARTUP_TIME} \
                    -JTG2_HOLD_TIME=${params.TG2_HOLD_TIME} \
                    -JTG2_SHUTDOWN_TIME=${params.TG2_SHUTDOWN_TIME} \
                    -JTG2_THROUGHPUT=${params.TG2_THROUGHPUT} \
                    -JTG3_START_THREADS=${params.TG3_START_THREADS} \
                    -JTG3_INITIAL_DELAY=${params.TG3_INITIAL_DELAY} \
                    -JTG3_STARTUP_TIME=${params.TG3_STARTUP_TIME} \
                    -JTG3_HOLD_TIME=${params.TG3_HOLD_TIME} \
                    -JTG3_SHUTDOWN_TIME=${params.TG3_SHUTDOWN_TIME} \
                    -JTG3_THROUGHPUT=${params.TG3_THROUGHPUT} \
                    -l jmeter.log
                    """

                    // Для Windows используйте bat, для Linux/Mac - sh
                    bat(jmeterCommand)
                }
            }
        }

        stage('Archive Logs') {
            steps {
                archiveArtifacts artifacts: 'jmeter.log', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            // Чтение и вывод логов в консоль Jenkins
            script {
                def logContent = readFile('jmeter.log')
                echo "=== JMeter Log ==="
                echo logContent
            }
        }
    }
}