node{
    stage('git clone'){
        git credentialsId: 'Git_Hub_Credentials', url: 'https://github.com/MahendranathreddyS/spring-boot-mongo-docker.git'
    }
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.3", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
    }
    stage('Build Docker Image'){
        sh 'docker build -t mahendranathreddy/spring-boot-mongo .'
    }
    stage('Push docker image'){
    withCredentials([string(credentialsId: 'Docker_Hub_PWD', variable: 'Docker_Hub_PWD')]) {
        sh "docker login -u mahendranathreddy -p '${Docker_Hub_PWD}'"
        sh 'docker push mahendranathreddy/spring-boot-mongo'
    }
    }
    stage('Remove docker image in jenkins'){
        sh 'docker rmi -f mahendranathreddy/spring-boot-mongo'
    }
    stage('Deploy in kubernetes'){
         kubernetesDeploy(
             configs:'springBootMongo.yml',
             kubeconfigId:'Kube_Config'
             )
    }
}
