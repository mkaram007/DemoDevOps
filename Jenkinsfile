pipeline{
    agent any

    stages{
        
        stage("init"){
            steps{
            git branch: "main", url:"https://github.com/mkaram007/DemoDevOps"
            }
        }

        stage("Build"){
            steps{
		sh "chmod u+x ./mvnw"
            	sh "./mvnw package"
	    }
        }
        stage("docker"){
            steps{
                sh "docker version"
                sh "docker build -t nexus.local:8082/java-maven-app:1.0 ."
                withCredentials([
		           usernamePassword(credentialsId: 'nexus', usernameVariable: 'USR', passwordVariable: 'PWD')
		    ]) {
		            sh "echo $PWD | docker login nexus.local:8082 -u $USR --password-stdin"
		            sh "docker push nexus.local:8082/java-maven-app:1.0"
		    }
            }
        }
    }
    post{
        success{
            echo "Deploying to tomcat"
            deploy adapters: [tomcat9(credentialsId: '7fbccacc-c7da-4d12-87c8-d421b41093c1', 
            path: '', 
            url: 'http://192.168.1.4:8080')], 
            contextPath: null, 
            war: '**/*.war'
        }
    }
}
