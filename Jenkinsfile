pipeline {
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git url:'https://github.com/Sehlor/CMS.git', branch:'master'
      }
    }
    
	stage("Build image") {
		steps {
			script {
				myapp = docker.build("sehlor/cms:${env.BUILD_ID}")
			}
		}
	}
    
	stage("Push image") {
		steps {
			script {
					docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
					myapp.push("latest")
					myapp.push("${env.BUILD_ID}")
				}
			}
		}
	}

    stage('Deploy App') {
      steps {
        script {
          sh "kubectl apply -f https://raw.githubusercontent.com/Sehlor/CMS/master/CMS.yml"
        }
      }
    }
  }
}
