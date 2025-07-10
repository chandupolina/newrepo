pipeline{
    agent any
    stages{
        stage ('BUILD'){
            steps{
                echo "********************Building the code**********************"
                sh 'rm -rf spring-petclinic'   
                sh 'git clone https://github.com/pavandath/spring-petclinic.git'
                dir ('spring-petclinic'){
                sh 'mvn clean package -DskipTests -Dcyclonedx.skip=true'
                stash name: 'build-jar', includes: 'target/*.jar'   
            }
            }
        }

        stage('CodeQuality'){
            steps{
                echo "**********RUNNING CODEQUALITY TEST***********"
                dir ('spring-petclinic'){
                sh '''
                mvn clean verify sonar:sonar \
                 -Dsonar.projectKey=pipeline \
                 -Dsonar.host.url=http://34.133.89.244/:9000 \
                 -Dsonar.login=sqp_fa848e5e27d3e210979de498901a0c464c0b948c\
                 -DskipTests \
                 -Dcyclonedx.skip=true 

                 '''
                }
            }
        }
    }
}
