@Library("shared-library") _

pipeline
{
    agent {
        docker {
            image 'ajsyd141/java-python'
            args '-u 0:0 --privileged --net host -v /var/run/docker.sock:/var/run/docker.sock -v /root/.m2:/root/.m2'
        }
    }

 stages {

    stage('Pipeline Metadata') {
        steps {
         script {
            gitCheckOut.customCheckout()
         }
      }  
    }
    stage('Install Dependencies'){
        steps{
            script{
                sh 'apk add --update --no-cache maven'
            }
        }
    }

    stage('Maven Build') {
        steps {
            mvnBuild()
        }  
    }

    stage('Unit Test') {
        steps {
            mvnTest()
        }
     }  

    stage('DockerImage Build') {
        steps {
             dockerBuild()
            }
        }
         
    stage('Docker Image Push') {
        steps {
            dockerImagePush(credentialsId: 'DockerID', url: 'https://registry.hub.docker.com')
        }
     }
}

post {
    always {
        cleanWs()
    }
}
 
}


