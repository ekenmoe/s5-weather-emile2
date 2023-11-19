
pipeline {

    agent any
    // agent {
    //     label ("aws-deploy")
    // }
    options {
        buildDiscarder(logRotator(numToKeepStr: '20'))
        disableConcurrentBuilds()
        timeout (time: 60, unit: 'MINUTES')
        timestamps()
    }
  
    environment {
            DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        }
    stages {
                stage('Login') {

                    steps {
                        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    }
                }


                // stage('Hello') {
                //     steps {
                //         sh '''
                //         ls 
                //         pwd
                //         '''
                //     }
                // }
        stage('Test-Auth-Binaries') {
         agent {
            docker {
              image 'golang:alpine'
              args '-u root:root'
            }
            }
            steps {
                sh '''
                cd weatherapp/auth/src/main
                go build
                go tess
                ls -latr
                cd -
                '''
            }
        }

       stage('Test-UI-Binaries') {
         agent {
            docker {
              image 'node:17'
              args '-u root:root'
            }
            }
            steps {
                sh '''
                cd weatherapp/UI
                npm run
                ls -latr
                cd -
                '''
            }
        }

//       stage('Test-Weather-Binaries') {
//          agent {
//             docker {
//               image 'python:3.8-slim-buster'
//               args '-u root:root'
//             }
//             }
//             steps {
//                 sh '''
//                 cd weatherapp/weather
//                 pip3 install -r requirements.txt
//                 ls -latr
//                 cd -
//                 '''
//             }
//         }

// stage('SonarQube analysis') {
//             agent {
//                 docker {
//                   image 'sonarsource/sonar-scanner-cli:4.7.0'
//                 }
//                }
//                environment {
//         CI = 'true'
//         //  scannerHome = tool 'Sonar'
//         scannerHome='/opt/sonar-scanner'
//     }
//             steps{
//                 withSonarQubeEnv('Sonar') {
//                     sh "${scannerHome}/bin/sonar-scanner"
//                 }
//             }
//         }
//     stage("Quality gate") {
//                 steps {
//                     timeout(time: 1, unit: 'HOURS') {
//                     waitForQualityGate abortPipeline: true }
//                 }
//             }

//       stage('Built-Auth-Image') {
        
//             steps {
//                 sh '''
//                 cd $WORKSPACE/weatherapp/auth
//                 docker build -t devopseasylearning/s5-emile-weatherapp-auth:${BUILD_NUMBER} .
//                 '''
//             }
//         }

//         stage("Push-Auth-Image") {
//                when{ 
//           expression {
//             env.GIT_BRANCH == 'origin/develop' }  
//             }
//             steps {
//                 sh '''
//                     docker push devopseasylearning/s5-emile-weatherapp-auth:${BUILD_NUMBER}
//                     '''
//                 }
//             }

//         stage('Built-UI-Image') {
                
//                     steps {
//                         sh '''
//                         cd $WORKSPACE/weatherapp/UI
//                         docker build -t devopseasylearning/s5-emile-weatherapp-ui:${BUILD_NUMBER} .
//                         '''
//                     }
//                 }

//         stage("Push-UI-Image") {
//                     when{ 
//                 expression {
//                     env.GIT_BRANCH == 'origin/develop' }  
//                     }
//                     steps {
//                         sh '''
//                             docker push devopseasylearning/s5-emile-weatherapp-ui:${BUILD_NUMBER}
//                             '''
//                         }
//                     }

//                   stage('Built-Weather-Image') {
        
//             steps {
//                 sh '''
//                 cd $WORKSPACE/weatherapp/weather
//                 docker build -t devopseasylearning/s5-emile-weatherapp-weather:${BUILD_NUMBER} .
//                 '''
//             }
//         }

//         stage("Push-Weather-Image") {
//                 when{ 
//             expression {
//                 env.GIT_BRANCH == 'origin/develop' }  
//                 }
//                 steps {
//                     sh '''
//                         docker push devopseasylearning/s5-emile-weatherapp-weather:${BUILD_NUMBER}
//                         '''
//                     }
//                 }

//           stage('Built-db-Image') {
        
//             steps {
//                 sh '''
//                 cd $WORKSPACE/weatherapp/db
//                 docker build -t devopseasylearning/s5-emile-weatherapp-db:${BUILD_NUMBER} .
//                 '''
//             }
//         }

//             stage("Push-DB-Image") {
//                         when{ 
//                     expression {
//                         env.GIT_BRANCH == 'origin/develop' }  
//                         }
//                         steps {
//                             sh '''
//                                 docker push devopseasylearning/s5-emile-weatherapp-db:${BUILD_NUMBER}
//                                 '''
//                             }
//                         }

//             stage('Built-redis-Image') {
            
//                 steps {
//                     sh '''
//                     cd $WORKSPACE/weatherapp/redis
//                     docker build -t devopseasylearning/s5-emile-weatherapp-redis:${BUILD_NUMBER} .
//                     '''
//                 }
//             }

//             stage("Push-redis-Image") {
//                     when{ 
//                 expression {
//                     env.GIT_BRANCH == 'origin/develop' }  
//                     }
//                     steps {
//                         sh '''
//                             docker push devopseasylearning/s5-emile-weatherapp-redis:${BUILD_NUMBER}
//                             '''
//                         }
//                     }

   
//         stage('Compose construct') {
//         agent { 
//             label "deployer"
//             }
//                 steps {
//                     script {
//                     withCredentials([
//                         string(credentialsId: 'WEATHERAPP_MYSQL_ROOT_PASSWORD', variable: 'WEATHERAPP_MYSQL_ROOT_PASSWORD'),
//                         string(credentialsId: 'WEATHERAPP_REDIS_PASSWORD', variable: 'WEATHERAPP_REDIS_PASSWORD'),
//                         string(credentialsId: 'WEATHERAPP_APIKEY', variable: 'WEATHERAPP_APIKEY'),
//                         string(credentialsId: 'WEATHERAPP_DB_PASSWORD', variable: 'WEATHERAPP_DB_PASSWORD')
//                     ]) {

//                         sh '''
//          cat <<EOF > docker-compose.yml
// version: '3.5'         
// services:
//   db:
//     container_name: weatherapp-db
//     image: devopseasylearning/s5-emile-weatherapp-db:${BUILD_NUMBER}
//     environment:
//       MYSQL_ROOT_PASSWORD: ${WEATHERAPP_MYSQL_ROOT_PASSWORD}
//     volumes:
//       - db-data:/var/lib/mysql
//     networks:
//       - weatherapp
//     restart: always

//   redis:
//     container_name: weatherapp-redis
//     image: devopseasylearning/s5-emile-weatherapp-redis:${BUILD_NUMBER}
//     networks:
//       - weatherapp
//     environment:
//       REDIS_USER: redis
//       REDIS_PASSWORD: ${WEATHERAPP_REDIS_PASSWORD}
//     volumes:
//       - redis-data:/data
//     restart: always

//   weather:
//     container_name: weatherapp-weather
//     image: devopseasylearning/s5-emile-weatherapp-weather:${BUILD_NUMBER}
//     expose:
//       - 5000
//     environment:
//       APIKEY: ${WEATHERAPP_APIKEY}
//     networks:
//       - weatherapp
//     restart: always
//     depends_on:
//       - db
//       - redis  # Weather depends on both db and redis
//   auth:
//     container_name: weatherapp-auth
//     image: devopseasylearning/s5-emile-weatherapp-auth:${BUILD_NUMBER}
//     environment:
//       DB_HOST: db
//       DB_PASSWORD: ${WEATHERAPP_DB_PASSWORD}
//     expose:
//       - 8080
//     networks:
//       - weatherapp
//     restart: always
//     depends_on:
//       - weather  # Auth depends on the weather service

//   ui:
//     container_name: weatherapp-ui
//     image: devopseasylearning/s5-emile-weatherapp-ui:${BUILD_NUMBER}
//     environment:
//      eric: eric
//      AUTH_HOST: auth
//      AUTH_PORT: 8080
//      WEATHER_HOST: weather
//      WEATHER_PORT: 5000
//      REDIS_USER: redis
//      REDIS_PASSWORD: redis
//     expose:
//       - 3000
//     ports:
//       - 3000:3000
//     networks:
//       - weatherapp
//     restart: always
//     depends_on:
//       - auth  # UI depends on Auth
// networks:
//   weatherapp:

// volumes:
//   db-data:
//   redis-data:
  
// EOF

//              '''
//                             }

//                             }

//                         }

//                         }

//          stage("Deploy") {
//                   	agent { 
//                     label "deployer"
//                     }
//                 when{ 
//                     expression {
//                     env.GIT_BRANCH == 'origin/develop' }  
//                     }
//                     steps {
//                         sh '''
//                           ls -ltrha
//                           pwd
//                           cat docker-compose.yml
//                           docker-compose down --remove-orphans
//                           docker-compose up -d
//                           docker-compose ps
//                           ip addr show 

//                             '''
//                         }
//                     }


    }
}


             

