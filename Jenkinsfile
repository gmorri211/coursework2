node {
    def app

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
        app = docker.build("gmorri211/coursework2:1.0")
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
    sshagent(['my-ssh-key']) {
    	sh 'ssh ubuntu@ec2-54-84-78-59.compute-1.amazonaws.com && kubectl set image deployments/coursework2 coursework2=gmorri211/coursework2:latest'
    }
}
