node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    registryHost = "registry.opsramp.net:8081/"
    imageName = "${registryHost}${appName}:latest"
    env.BUILDIMG=imageName

    stage "Build"
    
        sh "docker build -t ${imageName} -f applications/hello-kenzan/Dockerfile applications/hello-kenzan"
    
    stage "Push"

        sh "docker push ${imageName}"

    stage('deploying') {
    withKubeConfig([credentialsId: 'k8s-surya-ctl1', serverUrl: 'https://172.26.17.171:6443']) {
      sh 'kubectl delete -f "applications/${appName}/k8s/*.yaml"'
      sh 'kubectl create -f "applications/${appName}/k8s/*.yaml"'
    }
  }
}

 
