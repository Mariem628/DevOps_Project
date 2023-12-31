pipeline{
    agent any



    stages {


        stage('Getting project from Git') {
            steps{
      			checkout([$class: 'GitSCM', branches: [[name: '*/main']],
			extensions: [],
			userRemoteConfigs: [[url: 'https://github.com/Mariem628/DevOps_Project.git']]])
            }
        }


       stage('Cleaning the project') {
            steps{
                	sh "mvn -B -DskipTests clean  "
            }
        }



        stage('Artifact Construction') {
            steps{
                	sh "mvn -B -DskipTests package "
            }
        }



         stage('JUnit / Mockito') {
            steps{
               		 sh "mvn test "
            }
        }



        stage('SONARQUBE') {
            steps{

             		sh " mvn clean verify sonar:sonar -Dsonar.projectKey=DevOps_Project -Dsonar.projectName='DevOps_Project' -Dsonar.host.url=http://192.168.56.2:9000 -Dsonar.token=sqp_313fd4947b5c9b8ca0ba0214ca027607fd9955f8 "
            }
        }


        stage('Publish to Nexus') {
            steps {


  sh 'mvn deploy'


            }
        }

stage('Build Backend Docker Image') {
                      steps {
                          script {
                            sh 'docker build -t mariem15/spring-app:devops .'
                          }
                      }
                  }

                  stage('login dockerhub') {
                                        steps {
				sh 'docker login -u mariem15 --password dckr_pat_0iaom9peVjYUg0VIvUkeT-5V4bg'
                                            }
		  }

	                      stage('Push Backend Docker Image') {
                                        steps {
                                   sh 'docker push mariem15/spring-app:devops'
                                            }
		  }

 stage("clone frontend"){
         steps{
             script{
                   checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url:"https://github.com/mariem15/front.git"


]]])
             }
         }



 stage("build and push frontend docker image") {


            steps {
                script {

             sh 'docker login -u mariem15 --password dckr_pat_0iaom9peVjYUg0VIvUkeT-5V4bg'

             sh "docker push mariem15/front-app:devops"

                }
            }




            }


     }
	    stage('Build Frontend Docker Image') {
                      steps {
                          script {
                            sh 'docker build -t mariem15/front-app:devops .'
                          }
                      }
                  }

 stage("Push Frontend Docker image") {


            steps {
                script {

             sh 'docker login -u mariem15 --password dckr_pat_0iaom9peVjYUg0VIvUkeT-5V4bg'

             sh "docker push mariem15/front-app:devops"

                }
            }




            }
stage('Run Spring && MySQL Containers') {
                                steps {
                                    script {
                                      sh 'docker-compose up -d'
                                    }
                                }
                            }




}




}
