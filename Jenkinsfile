pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ZiskaS/threepoints_devops_webserver.git'
            }
        }
        
        stage('Pruebas de SAST') {
            steps {
                sh 'echo "Ejecución de pruebas de SAST"'
            }
        }
        
        stage('Creación de archivo de credenciales') {
            steps {
                script {
                    sh 'sed s/${USERNAME}/@credentials(name=USERNAME,defaultValue=)/ credentials.ini.tpl > credentials.ini'
                    sh 'sed -i s/${PASSWORD}/@credentials(name=PASSWORD,defaultValue=)/ credentials.ini'
                }
            }
        }
        
        stage('Construcción del contenedor de Docker') {
            steps {
                script {
                    def tag = "devops_ws_${System.currentTimeMillis()}"
                    sh "docker build -t devops_ws . --tag $tag"
                }
            }
        }
    }
    
    options {
        skipDefaultCheckout() // Opcional, evita que se realice un checkout automático
    }
    
    triggers {
        cron('@daily') // Opcional, para programar la ejecución periódica del pipeline
    }
}
