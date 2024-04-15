pipeline {
    agent any
    
    stages {
        stage('Pruebas de SAST') {
            steps {
                sh 'echo "Ejecución de pruebas de SAST"'
            }
        }
        
        stage('Creación de archivo de credenciales') {
            steps {
                script {
                    def username = credentials('USERNAME')
                    def password = credentials('PASSWORD')
                    
                    sh "sed 's/\${USERNAME}/${username}/' credentials.ini.tpl | sed 's/\${PASSWORD}/${password}/' > credentials.ini"
                }
            }
        }
        
        stage('Construcción del contenedor de Docker') {
            steps {
                sh 'docker build -t devops_ws . --tag devops_ws_$(date +%s)'
            }
        }
    }
}

def credentials(String name) {
    return credentials(name: name, defaultValue: '')
}
