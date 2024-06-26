stages {
        stage('Checkout') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-username-password', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    git url: 'https://github.com/ZiskaS/threepoints_devops_webserver.git', branch: 'master', credentialsId: 'github-username-password'
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
                    sh "sed -e 's/\${USERNAME}/${env.USERNAME}/g' -e 's/\${PASSWORD}/${env.PASSWORD}/g' credentials.ini.tpl > credentials.ini"
                }
            }
        }

        stage('Crear y empujar ramas') {
            steps {
                script {
                    sh "git checkout -b feature/nombre_de_la_feature"
                    sh "git push origin feature/nombre_de_la_feature"
                    
                    sh "git checkout -b hotfix/nombre_del_hotfix"
                    sh "git push origin hotfix/nombre_del_hotfix"
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
