pipeline {
    agent any
    
    environment{
        SONARQUBE_ENV='MySonar'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/krunal1975/python_Jenkins_sonarProject.git',branch: 'main'
            }
        }
        stage('Create Virtualenv') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests & Coverage') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest --cov=app --cov-report=xml
                '''
            }
        }

        stage('SonarQube Analysis'){
            steps{
                withSonarQubeEnv("${SONARQUBE_ENV}"){
                    sh '''
                        . venv/bin/activate
                        /opt1/sonar-scanner/bin/sonar-scanner \
                    '''
                }
            }
        }
        
        stage('Quality Gate'){
            steps{
                timeout(time: 2, unit: 'MINUTES'){
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}