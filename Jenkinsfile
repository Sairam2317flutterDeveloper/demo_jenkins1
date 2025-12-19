pipeline {
    agent any
    tools{
        maven "M3"
    }
    triggers{
        cron("22 21 * * *")
    }
    environment{
        DOKER_IMAGE = "jenkins:demo1"
        DOKER_CONTAINER = "DemoContainer"
    }

    stages {
            stage("GIt Clone") {
            steps {
                git branch: "main",
                url: "https://github.com/Sairam2317flutterDeveloper/demo_jenkins1.git"
            }
        }
             stage("Mvn build "){
                 steps{
                     sh "mvn clean package"
                 }
            
        }
        stage("Sonar Analysis"){
            steps{
               sh """ mvn clean verify sonar:sonar \
                -Dsonar.projectKey=jenkins_demo \
               -Dsonar.host.url=http://localhost:9000 \
               -Dsonar.login=sqp_140fef4aa765146d066335a09d7e3b787520fdcf """
            }
        }
        stage("Build Docker Image"){
            steps{
                sh "docker build -t $DOKER_IMAGE ."
            }
        }
         stage("Build Docker Container"){
            steps{
                sh "docker stop  $DOKER_CONTAINER|| true "
                sh "docker rm $DOKER_CONTAINER|| true "
                sh "docker run -d -p 8383:8080 --name $DOKER_CONTAINER $DOKER_IMAGE"
            }
        }
    }
    post {
        always{
            cleanWs()
        }
    }
}
