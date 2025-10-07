pipeline{
    agent{
        node{
            label 'maven'
        }
    }

    stages{
        stage('Test'){
            steps{
                sh './mvnw clean test'
            }
        }
        stage('Package test'){
            steps{
                sh '''
                    ./mvnw package -DskipTests \
                    -Dquarkus.package.type=uber-jar
                    '''
                archiveArtifacts 'target/*.jar'
            }
        }
    }





}