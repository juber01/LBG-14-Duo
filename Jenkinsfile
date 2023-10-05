pipeline{
        agent any
        stages{
            stage('Build Images'){
                steps{
                    sh '''
                    docker build -t juber81/duo-flask-app:latest -t juber81/duo-flask-app:v${BUILD_NUMBER} .
                    docker build -t juber81/duo-mynginx:latest -t juber81/duo-mynginx:v${BUILD_NUMBER} ./nginx
                    '''
                }
            }
            stage('Push Images'){
                steps{
                    sh '''
                    docker push juber81/duo-flask-app:latest
                    docker push juber81/duo-flask-app:v${BUILD_NUMBER}
                    docker push juber81/duo-mynginx:latest
                    docker push juber81/duo-mynginx:v${BUILD_NUMBER}
                    '''
                }
            }
            stage('Cleanup Jenkins'){
                steps{
                    sh '''
                    docker rmi juber81/duo-flask-app:latest
                    docker rmi juber81/duo-flask-app:v${BUILD_NUMBER}
                    docker rmi juber81/duo-mynginx:latest
                    docker rmi juber81/duo-mynginx:v${BUILD_NUMBER}
                    '''
                }
            }
            stage('Deploy Containers'){
                steps{
                    sh '''
                    kubectl apply -f ./k8s/ -n production
                    kubectl rollout restart deployment flask-deployment -n production
                    '''
                }
            }
        }
}