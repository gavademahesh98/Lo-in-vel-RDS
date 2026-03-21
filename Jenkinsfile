pipeline{

    agent any

    stages{


        stage('git checkout'){

            steps{

                git branch: 'master',
                url: 'https://github.com/gavademahesh98/log-in-vel.git'
            }
        }

        stage('Build'){

            steps{

            sh  'docker build -t maheshg98/mylogin:${BUILD_NUMBER} .'
                    
             }
        }
        stage('push-image'){
            steps{

            sh """
                  docker login -u maheshg98 -p Mahesh@8798
                  docker push maheshg98/mylogin:${BUILD_NUMBER}
            """
            }
        }
        stage('deployment'){

            steps{
                sh """
                    docker pull maheshg98/mylogin:${BUILD_NUMBER}
                    docker kill login-app || true
                    docker container rm login-app || true
                    docker run -itd -p 4545:8080 --name login-app maheshg98/mylogin:${BUILD_NUMBER}
                    
                    docker images maheshg98/mylogin --format "{{.Repository}}:{{.Tag}}" \
                    | tail -n +4 \
                    | xargs -r docker rmi

                    # create SQl database container and run
                    docker kill mysql || true
                    docker container rm mysql || true
                    docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=test -p 3000:3306 mysql:latest

                """
            }
        }






    }
}