pipeline {

  agent any 
  
  stages {
    
    stage("build") {
      steps {
         configFileProvider([configFile(fileId: 'app-documents-service-api', variable: 'CONFIG')]) {
           echo 'building the applications...'
           sh 'mvn clean package'
           sh 'echo $CONFIG.name json'
        }
        
      }
    }
    
     stage("build-docker-image") {
      steps {
        echo 'building the docker image...'
        sh 'docker build -t oiestradag/app-tra-documents-service-api .'
      }
    }
    
    
    stage("push-dockerhub") {
      steps {
          withCredentials([string(credentialsId: 'docker_hub_oiestradag_pass', variable: 'pass')]){
              sh 'docker login -u oiestradag -p "$pass"'
              sh 'docker push oiestradag/app-tra-documents-service-api'
          }
       }
    }
    
    
    
    
    stage("test") {
      steps {
        echo 'testing the applications...'
      }
    }
    
    
    stage("deploy") {
      steps {
        echo 'deploying the applications...'
        sh 'docker rm -f  app-tra-documents-service-api || true'
        sh 'docker run -e PORT=80 -e APP_VERSION=develop -e SQL_URL_CONECTION=jdbc:h2:mem:testdb -e SQL_USERNAME=sa -e SQL_PASSWORD=password -tid --name app-tra-documents-service-api -p 8082:80 oiestradag/app-tra-documents-service-api /bin/bash'
      }
    }
    
    /*stage("k8s deploy") {
      steps {
        echo 'deploying the applications...'
        sh 'kubectl apply -f app-k8s.yaml'
      }
    }*/
    
    
  }

}
