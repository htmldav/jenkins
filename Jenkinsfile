pipeline {
    agent { 
        docker { 
            image 'aldav99/ssh-test:v170304'
            args '-u root:root -v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker'
        }
    }
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello, Maven'
                sh 'mvn --version'
                
                echo 'Hello, openssh-client'
                sh 'ssh -V'
            }
        }
        stage('delete test') {
            steps {
                sh 'rm -rf test'
            }
        }
        stage('Copy source') {
            steps {
                echo 'Copy source'    
                sh 'git clone https://github.com/htmldav/test.git'
            }
        }
        stage('Build war') {
            steps {
                sh 'cd test && mvn package'
            }
        }
        
        stage('Test && Make docker image') {
            steps {
                sh 'cd test && cat Dockerfile && docker build -t testdind . && docker tag testdind aldav99/testdind && echo "Gfdkjdcrfz25" | docker login --username aldav99 --password-stdin docker.io && docker push aldav99/testdind'
            }
        }
        
        stage('Pull image') {
            steps {
                sh 'ssh-keyscan -H 130.193.55.125 >> ~/.ssh/known_hosts'
                sh 'ssh root@130.193.55.125'
                sh '''ssh root@130.193.55.125 << EOF
	docker pull aldav99/testdind
	docker run -d -p 8080:8080 aldav99/testdind
EOF'''
            }
        }
    }
}
