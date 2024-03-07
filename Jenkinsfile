pipeline{
    agent any;

    stages{
        stage('verify kubectl installed or not '){
            steps{
                sh''' 
                    kubectl version
                    if [ $? == 0]
                    then
                        echo "kubectl installed"
                    else
                        exit 1
                    fi    
                '''
            }
        }
        stage('connectivity check to eks'){
                steps{
                    ''' kubectl get nodes
                        if [ $? == 0]
                        then
                            echo "connection established"
                        else
                           exit 1
                        fi 
                    '''
                }
            }
        stage('deploy the mariadb database'){
                steps{
                    sh'''
                        helm upgrade --install devops-mariadb mariadb --values mariadb/createat-mariadb.yaml
                        sleep 30s
                        helm status devops-mariadb mariadb
                    '''
                }
            }
        stage('Dockerfile to build image for java') {
            steps{
                sh '''  cd springboot/java-springboot
                        bash Build.sh
                        #docker build -t ram333/createat-devops-task:java-spring-v1 .
                        #docker push ram333/createat-devops-task:java-spring-v1 
                '''

            }
        }   
        stage('deploy the springboot'){
                steps{
                    sh'''
                        helm upgrade --install devops-spring springboot --values springboot/create-task-java.yaml
                        sleep 30s
                        helm status devops-spring springboot
                    '''
                }
            }
       /* stage('deploy the reactjs'){
                steps{
                    sh'''
                        kubectl apply -f react.yaml
                        sleep 10s
                        kubectl apply -f svc.yaml
                    '''
                }
           }
           */
           
    }
}
