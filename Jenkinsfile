pipeline {
  agent any

  environment {
    DOCKER_REGISTRY="058331092577.dkr.ecr.ap-south-1.amazonaws.com"
    K8S_NAMESPACE = 'backend'
    K8S_DEPLOYMENT_NAME = 'project'
  }

  stages {
    stage('Build Docker Image') {
      steps {
        sh '''
	 whoami
         aws configure set aws_access_key_id AKIAQ3FGN2ZQWYGWBDES
         aws configure set aws_secret_access_key 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
         aws configure set default.region ap-south-1
         #echo $AWS_ACCESS_KEY_ID
         #DOCKER_LOGIN_PASS=$(aws ecr get-login-password  --region us-east-1
         #docker login -u AWS -p $DOCKER_LOGIN_PASS https://058331092577.dkr.ecr.ap-south-1.amazonaws.com/project1
         aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 058331092577.dkr.ecr.ap-south-1.amazonaws.com
	 docker build -t 058331092577.dkr.ecr.ap-south-1.amazonaws.com/project1:SAMPLE-PROJECT-${BUILD_NUMBER} .
         docker push 058331092577.dkr.ecr.ap-south-1.amazonaws.com/project1:SAMPLE-PROJECT-${BUILD_NUMBER}
	  '''
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        sh '''
            sed "s/buildNumber/${BUILD_NUMBER}/g" K8/deployment.yaml > deployment-new.yaml
            kubectl apply -f deployment-new.yaml -n $K8S_NAMESPACE
            kubectl apply -f service.yaml -n $K8S_NAMESPACE
           '''
      }
    }
  }
}
