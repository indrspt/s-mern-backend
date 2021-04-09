env.DOCKER_REGISTRY = 'indraraspati'
env.DOCKER_IMAGE_NAME = 's-mern-backend'
node('master') {
	stage('s-Mern Backend') {
      echo 's-Mern Backend'
    }
    stage('Git Clone from Github') {
        git url: 'https://github.com/indrspt/s-mern-backend.git'
    }
    stage('Build Docker Image') {
        sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER} ."
    }
    stage('Push Docker Image to Docker Hub') {
        sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage('Deploy To Kubernetes Cluster') {
        sh'''sed -i "20d" s-backend-deploy.yml'''
        sh'''sed -i "19 a \'\\'        image: indraraspati/s-mern-backend:${BUILD_NUMBER}" s-backend-deploy.yml && sed -i "s/''//"  s-backend-deploy.yml'''
        sh "kubectl apply -f  s-backend-deploy.yml"
   }
    stage('Remove Docker Image in local') {
        sh "docker rmi $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:${BUILD_NUMBER}"
    }
    stage("Clean Workspace"){
        cleanWs()
    }
}
