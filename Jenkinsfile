def containerName="springbootdocker"
def tag="assm2"
def dockerHubAccount="kietdo"
def dockerUser="kietdo"
def dockerPassword="Godlike0710"
def gitURL="https://github.com/kietdo0710/SpringBootDocker.git"

node {
    stage('Checkout') {
        checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: gitURL]]]
    }

    stage('Build'){
        sh "mvn clean install"
    }

    stage("Image Prune"){
         sh "docker image prune -f"
    }

    stage('Image Build'){
        sh "docker build -t $containerName:$tag --pull --no-cache ."
        echo "Image build complete"
    }

    stage('Push to Docker Registry'){
	    withCredentials([usernamePassword(credentialsId: 'MyCreds', usernameVariable: dockerUser, passwordVariable: dockerPassword)]) {
            sh "docker login -u $dockerUser -p $dockerPassword"
            sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
            sh "docker push $dockerUser/$containerName:$tag"
            echo "Image push complete"
        }
    }    
}
