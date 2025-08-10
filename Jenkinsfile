pipeline {
    agent any
    
    parameters {
        // Общие параметры
        string(name: 'JMETER_TEST_PLAN', defaultValue: 'path/to/your/test.jmx', description: 'Путь к JMX файлу теста')
        string(name: 'JMETER_HOME', defaultValue: env.JMETER_HOME, description: 'Путь к JMeter')
        string(name: 'RESULTS_DIR', defaultValue: 'results', description: 'Директория для результатов')
        
        // Параметры для Thread Group 1
        string(name: 'TG1_START_THREADS', defaultValue: '300', description: 'TG1: Начальное количество потоков')
        string(name: 'TG1_INITIAL_DELAY', defaultValue: '0', description: 'TG1: Начальная задержка (сек)')
        string(name: 'TG1_STARTUP_TIME', defaultValue: '720', description: 'TG1: Время наращивания нагрузки (сек)')
        string(name: 'TG1_HOLD_TIME', defaultValue: '14400', description: 'TG1: Время удержания нагрузки (сек)')
        string(name: 'TG1_SHUTDOWN_TIME', defaultValue: '0', description: 'TG1: Время снижения нагрузки (сек)')
        string(name: 'TG1_THROUGHPUT', defaultValue: '60', description: 'TG1: Пропускная способность (запросов/мин)')
        
        // Параметры для Thread Group 2
        string(name: 'TG2_START_THREADS', defaultValue: '300', description: 'TG2: Начальное количество потоков')
        string(name: 'TG2_INITIAL_DELAY', defaultValue: '0', description: 'TG2: Начальная задержка (сек)')
        string(name: 'TG2_STARTUP_TIME', defaultValue: '720', description: 'TG2: Время наращивания нагрузки (сек)')
        string(name: 'TG2_HOLD_TIME', defaultValue: '14400', description: 'TG2: Время удержания нагрузки (сек)')
        string(name: 'TG2_SHUTDOWN_TIME', defaultValue: '0', description: 'TG2: Время снижения нагрузки (сек)')
        string(name: 'TG2_THROUGHPUT', defaultValue: '60', description: 'TG2: Пропускная способность (запросов/мин)')
        
        // Параметры для Thread Group 3
        string(name: 'TG3_START_THREADS', defaultValue: '300', description: 'TG3: Начальное количество потоков')
        string(name: 'TG3_INITIAL_DELAY', defaultValue: '0', description: 'TG3: Начальная задержка (сек)')
        string(name: 'TG3_STARTUP_TIME', defaultValue: '720', description: 'TG3: Время наращивания нагрузки (сек)')
        string(name: 'TG3_HOLD_TIME', defaultValue: '14400', description: 'TG3: Время удержания нагрузки (сек)')
        string(name: 'TG3_SHUTDOWN_TIME', defaultValue: '0', description: 'TG3: Время снижения нагрузки (сек)')
        string(name: 'TG3_THROUGHPUT', defaultValue: '60', description: 'TG3: Пропускная способность (запросов/мин)')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/NekiyTot/firstReposit.git'
            }
        }
        
        stage('Run JMeter Test') {
            steps {
                script {
                    // Создаем директорию для результатов
                    bat """
                        if not exist "${WORKSPACE}/${params.RESULTS_DIR}" mkdir "${WORKSPACE}/${params.RESULTS_DIR}"
                    """
                    
                    // Запускаем JMeter с передачей параметров через -J
                    bat """
                        "${params.JMETER_HOME}\\bin\\jmeter.bat" -n ^
                            -t "${WORKSPACE}/${params.JMETER_TEST_PLAN}" ^
                            -l "${WORKSPACE}/${params.RESULTS_DIR}/result.jtl" ^
                            -JTG1_START_THREADS=${params.TG1_START_THREADS} ^
                            -JTG1_INITIAL_DELAY=${params.TG1_INITIAL_DELAY} ^
                            -JTG1_STARTUP_TIME=${params.TG1_STARTUP_TIME} ^
                            -JTG1_HOLD_TIME=${params.TG1_HOLD_TIME} ^
                            -JTG1_SHUTDOWN_TIME=${params.TG1_SHUTDOWN_TIME} ^
                            -JTG1_THROUGHPUT=${params.TG1_THROUGHPUT} ^
                            -JTG2_START_THREADS=${params.TG2_START_THREADS} ^
                            -JTG2_INITIAL_DELAY=${params.TG2_INITIAL_DELAY} ^
                            -JTG2_STARTUP_TIME=${params.TG2_STARTUP_TIME} ^
                            -JTG2_HOLD_TIME=${params.TG2_HOLD_TIME} ^
                            -JTG2_SHUTDOWN_TIME=${params.TG2_SHUTDOWN_TIME} ^
                            -JTG2_THROUGHPUT=${params.TG2_THROUGHPUT} ^
                            -JTG3_START_THREADS=${params.TG3_START_THREADS} ^
                            -JTG3_INITIAL_DELAY=${params.TG3_INITIAL_DELAY} ^
                            -JTG3_STARTUP_TIME=${params.TG3_STARTUP_TIME} ^
                            -JTG3_HOLD_TIME=${params.TG3_HOLD_TIME} ^
                            -JTG3_SHUTDOWN_TIME=${params.TG3_SHUTDOWN_TIME} ^
                            -JTG3_THROUGHPUT=${params.TG3_THROUGHPUT}
                    """
                }
            }
        }
        
        stage('Generate Report') {
            steps {
                script {
                    // Генерируем отчет
                    bat """
                        "${params.JMETER_HOME}\\bin\\jmeter.bat" -g ^
                            "${WORKSPACE}/${params.RESULTS_DIR}/result.jtl" ^
                            -o "${WORKSPACE}/${params.RESULTS_DIR}/report"
                    """
                }
            }
        }
        
        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: "${params.RESULTS_DIR}/**", allowEmptyArchive: true
                junit testResults: "${params.RESULTS_DIR}/*.jtl", allowEmptyResults: true
                
                // Публикуем HTML отчет
                publishHTML target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: "${params.RESULTS_DIR}/report",
                    reportFiles: 'index.html',
                    reportName: 'JMeter HTML Report'
                ]
            }
        }
    }
    
    post {
        always {
            // Очистка временных файлов
            deleteDir()
            
            // Уведомление о завершении
            emailext (
                subject: "JMeter Test ${currentBuild.currentResult}",
                body: """
                    Тест завершен со статусом: ${currentBuild.currentResult}
                    Параметры теста:
                    TG1: ${params.TG1_START_THREADS} потоков, ${params.TG1_HOLD_TIME} сек удержания
                    TG2: ${params.TG2_START_THREADS} потоков, ${params.TG2_HOLD_TIME} сек удержания
                    TG3: ${params.TG3_START_THREADS} потоков, ${params.TG3_HOLD_TIME} сек удержания
                """,
                to: 'your-email@example.com'
            )
        }
    }
}