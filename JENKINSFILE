pipeline {
    agent any
    stages {
        stage('Get Code') {
            steps {
                // Obtener código del repo
                git 'https://github.com/darioreyesr25/unir-CP1.git'
                bat 'dir'
                echo WORKSPACE
            }
        }
    
        stage('Build') {
            steps {
                echo 'Eyyy, esto es Python. No hay que compilar nada!!!'
                echo WORKSPACE
                bat 'dir'
            }
        }
    
        stage('Unit') {
            steps {
                bat '''
                    set PYTHONPATH=%WORKSPACE%
                    pytest --junitxml=result-unit.xml test\\unit
                    '''
                    
                }
        }
        
        
        stage('Rest') {
            steps {
                catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                    bat '''
                    set FLASK_APP=app\\api.py
                    start flask run
                    start java -jar C:\\proyectos\\wiremock-standalone-3.13.2.jar --port 9090 --root-dir C:\\proyectos\\mappings

                    ping -n 10 127.0.0.1

                    pytest --junitxml=result-rest.xml test\\rest
                    '''
                    
                }
            }
        }
        
        stage('Result') {
            steps {
                junit 'result*.xml'
            }
        }
     }
}