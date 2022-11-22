pipeline {

	agent any

	stages {
	
		stage ('Git clonning') {
			
			agent {
				
				node {		
					label 'agent1'
					customWorkspace '/mnt/agent1'
				}
			}	
			
			steps {			
					sh 'yum install git -y'
					git 'https://github.com/Dee601/webapp.git'
					sh 'ls'
					//sh 'cd ..'
					sh 'pwd'
			}
		}		
			
		stage ('install maven, tomcat, jenkins') {
			
			agent {
				
				node {			
					label 'agent1'
					customWorkspace '/mnt/agent1'
				}
			}	
			
			steps {
						
					sh 'mkdir servers'
					//sh 'cd /mnt/agent1/servers'
					sh 'ls'
					sh '''cd /mnt/agent1/servers/
					wget https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.zip
					wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.68/bin/apache-tomcat-9.0.68-windows-x64.zip
					ls
					unzip apache-maven-3.8.6-bin.zip
					unzip apache-tomcat-9.0.68-windows-x64.zip
					ls
					rm -rf *zip
					ls
					chmod -R 777 apache-tomcat-9.0.68/bin/*'''
					
					sh '''echo "export MAVEN_HOME=/mnt/agent1/servers/apache-maven-3.8.6" >> /root/.bashrc
					echo "export PATH=/mnt/agent1/servers/apache-maven-3.8.6/bin:$PATH" >> /root/.bashrc
					echo "export M2_HOME=/mnt/agent1/servers/apache-maven-3.8.6" >> /root/.bashrc
					echo "export M2=/mnt/agent1/servers/apache-maven-3.8.6/bin" >> /root/.bashrc'''					
					
					//sh 'echo "export PATH" >> /root/.bash_profile'
					sh 'source /root/.bashrc'
					sh '''cd /mnt/agent1/servers/apache-tomcat-9.0.68/webapps
					wget https://get.jenkins.io/war-stable/2.346.1/jenkins.war'''
					sh 'echo "/mnt/agent1/servers/apache-tomcat-9.0.68/bin/startup.sh" >> /root/.bashrc'
					sh 'source /root/.bashrc'
			}		
		}				
					
		stage ('install httpd') {
			
			agent {
				
				node {	
					label 'agent1'
					customWorkspace '/mnt/agent1'
				}
			}			
			
			steps {
					sh 'sudo yum install httpd -y'
					sh 'systemctl start httpd'
					sh 'systemctl status  httpd'
					sh 'echo "<h1>Successfully installed HTTPD on $HOSTNAME</h1>" >> /var/www/html/index.html'
		
			}
		
		}
		
		stage ('build & deploy') {
			
			agent {
			
				node {
					label 'agent1'
					customWorkspace '/mnt/agent1'
				}
			}

			tools {
				maven 'maven-node1'
			}
			
			steps {
				sh 'mvn clean install'
				sh 'cp target/WebApp.war /mnt/agent1/servers/apache-tomcat-9.0.68/webapps'
			}
		
		}
		
		stage('take backup on S3') {
		
			agent {
					label 'agent1'
			}
			
			environment { 
                AN_ACCESS_KEY = credentials('aws-cli') 
            }
		
			steps {
				echo '1st configure CLI using "aws configure" command'
				sh 'aws s3 mb s3://demo21-11-22'
				sh 'aws s3 ls'
				sh 'aws s3 cp --recursive /mnt/agent1 s3://demo21-11-22/backup'
				sh 'aws s3 ls s3://demo21-11-22'
			}
		
		}
		
	}

}
