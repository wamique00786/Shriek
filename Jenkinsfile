Pipeline{
  agen{
    any
  }
  stages{
    stage('Clone Repo'){
      step{
        sh ' git clone -b main https://github.com/wamique00786/animalrescuer.git'
      }
    }
    stage('Build'){
      step{
        sh '''
        docker build -t wamique00786/animalrescuer:${TimeStamp} .
        docker tag wamique00786/animalrescuer:${TimeStamp} wamique00786/animalrescuer:latest
        '''
      }
    }
    stage('Pushing artifact'){
      step{
        sh '''
        docker login -u dockeruser -p dockerpassword
        docker push  wamique00786/animalrescuer:latest
        docker push wamique00786/animalrescuer:${TimeStamp}
        '''
      }
    }
     stage('UAT Deployment'){
      step{
        sh '''
        docker login -u dockeruser -p dockerpassword
        docker pull  wamique00786/animalrescuer:latest
        '''
      }
       script {
                    // Check if the container is running, stop and remove it if it exists
                    def containerExists = sh(
                        script: "docker ps -q -f name=animalrescuer",
                        returnStatus: true
                    ) == 0

                    if (containerExists) {
                        echo 'Stopping and removing existing container...'
                        sh "docker stop animalrescuer"
                        sh "docker rm animalrescuer"
                    } else {
                        echo 'No existing container to stop.'
                    }

                    // Run the new container
                    echo 'Starting new container...'
                    sh "docker run -d --rm --restart=always --name animalrescuer -p 80:8000 wamique00786/animalrescuer:latest"
                }
    }
     stage('Production Deployment'){
      step{
        sh '''
        docker login -u dockeruser -p dockerpassword
        docker pull  wamique00786/animalrescuer:latest
        docker run -d --rm --restart=always --name animalrescuer -p80:8000 wamique00786/animalrescuer:latest
        '''
      }
    }
