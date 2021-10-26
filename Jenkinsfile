pipeline {
    agent any	
    stages {
	    stage('Checkout') {
		    steps {
			    git credentialsId: 'Github_credentials', url: 'https://github.com/arshad-ahmad/django-postgres-deployment-on-eks-through-jenkins'
			    checkout scm
		    }
	    }
	    stage('Build image') {
		    steps {
			    script {
				    app = docker.build("arshad1914/pipeline:${env.BUILD_ID}")
		    	    }
		    }
	    }
	    
	    stage('Push image') {
		    steps {
			    script {
				    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
					    sh "docker login -u arshad1914 -p ${dockerhub}"
				    }
				    app.push("${env.BUILD_ID}")
			     }
							     
		    }
	    }
	 
	    stage('Deploy to K8s') {
		    steps{
			    echo "Deployment started ..."
			    sh 'ls -ltr'
			    sh 'pwd'
			    sh "sed -i 's/pipeline:latest/pipeline:${env.BUILD_ID}/g' deployment.yaml"
                            sh "kubectl -f apply deployment.yaml"
	    		}
        	}
    	}    
}
