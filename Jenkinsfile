import groovy.json.JsonSlurperClassic
def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
pipeline {
    agent any
    stages {
        stage("Paso 0: Download and checkout"){
            steps {
                checkout(
                    [$class: 'GitSCM',
                    //Acá reemplazar por el nonbre de branch
                    branches: [[name: "feature-nexus" ]],
                    //Acá reemplazar por su propio repositorio
                    userRemoteConfigs: [[url: 'https://github.com/luisahernandez/ejemplo-maven.git']]])
            }
        }
        stage("Paso 1: Compilaar"){
            steps {
                script {
                sh "echo 'Compile Code!'"
                // Run Maven on a Unix agent.
                sh "mvn clean compile -e"
                }
            }
        }
        stage("Paso 2: Testear"){
            steps {
                script {
                sh "echo 'Test Code!'"
                // Run Maven on a Unix agent.
                sh "mvn clean test -e"
                }
            }
        }
        stage("Paso 3: Build .Jar"){
            steps {
                script {
                sh "echo 'Build .Jar!'"
                // Run Maven on a Unix agent.
                sh "mvn clean package -e"
                }
            }
        }
        
		
        stage("Run: Levantar Springboot APP"){
            steps {
                sh 'nohup bash java -jar DevOpsUsach2020-0.0.8.jar & >/dev/null'
            }
        }
        stage("Curl: Dormir(Esperar 20sg) "){
            steps {
               sh "newman run ejemplo-maven.postman_collection.json  -n 10  --delay-request 1000'"
            }
        }
        
    }
    post {
        always {
            sh "echo 'fase always executed post'"
        }
        success {
            sh "echo 'fase success'"
        }
        failure {
            sh "echo 'fase failure'"
        }
    }
}