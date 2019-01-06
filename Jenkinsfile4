pipeline {
	    agent any
	
	    parameters {
	         string(name: 'tomcat_dev', defaultValue: '172.31.5.7', description: 'Staging Server')
	         string(name: 'tomcat_prod', defaultValue: '172.31.2.145', description: 'Production Server')
	    }
	
	    triggers {
	         pollSCM('* * * * *')
	     }
	
	stages{
	        stage('Build'){
	            steps {
	                sh 'mvn clean package'
	            }
	            post {
	                success {
	                    echo 'Now Archiving...'
	                    archiveArtifacts artifacts: '**/target/*.war'
	                }
	            }
	        }
	
	        stage ('Deployments'){
	            parallel{
	                stage ('Deploy to QA'){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/Pipeline_As_Code_Job/target/vprofile-v1.war jenkins@${params.tomcat_dev}:/usr/local/apache-tomcat-8.5.34/webapps"
	                    }
	                }
	
	                stage ("Deploy to Production"){
	                    steps {
	                        sh "scp -p -r /var/lib/jenkins/workspace/Pipeline_As_Code_Job/target/vprofile-v1.war jenkins@${params.tomcat_prod}:/usr/local/apache-tomcat-8.5.34/webapps"
	                    }
	                }
	            }
	        }
	    }
	}
