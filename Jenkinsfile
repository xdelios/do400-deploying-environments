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
        stage('Build image'){
            environment { QUAY = credentials('QUAY_USER')}
            steps{
                sh '''
                    ./mvnw quarkus:add-extension \
                    -Dextensions="kubernetes,container-image-jib"
                    '''
                sh '''
                    ./mvnw package -DskipTests \
                    -Dquarkus.jib.base-jvm=quay.io/redhattraingin/do400-java-alpine-openjdk11-jre:latest \
                    -Dquarkus.container-image.build=true \
                    -Dquarkus.container-image.registry=quay.io \
                    -Dquarkus.container-image.group=$QUAY_USR \
                    -Dquarkus.container-image.name=do400-deploying-environments \
                    -Dquarkus.container-image.username=$QUAY_USR \
                    -Dquarkus.container-image.password="$QUAY_PSW" \
                    -Dquarkus.container-image.push=true
                    '''
            }
        }
    }





}