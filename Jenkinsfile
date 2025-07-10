pipeline {
    agent {
        label 'java-slave'
    }

    tools {
        jdk 'jdk-21'
    }

    stages {
        stage ('Checkout') {
            steps {
                echo "********************Cloning the code**********************"
                // Clean up previous workspace and clone the repository
                deleteDir() 
                git 'https://github.com/pavandath/spring-petclinic.git'
            }
        }

        stage('Build & Analyze') {
            steps {
                dir('spring-petclinic') {
                    // This ensures the JDK from the 'tools' section is used for this step
                    withEnv(["JAVA_HOME=${tool 'jdk-21'}", "PATH=${tool 'jdk-21'}/bin:${env.PATH}"]) {
                        
                        // Add a command to verify the java version being used
                        echo "**********VERIFYING JAVA VERSION***********"
                        sh 'java -version'

                        echo "**********BUILDING & RUNNING CODEQUALITY TEST***********"
                        // Combined build, verification, and Sonar analysis in one command
                        // 'verify' runs the compile and package phases before 'sonar:sonar'
                        sh '''
                        mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=pipeline \
                            -Dsonar.host.url=http://34.133.89.244:9000 \
                            -Dsonar.login=sqp_fa848e5e27d3e210979de498901a0c464c0b948c \
                            -DskipTests \
                            -Dcyclonedx.skip=true
                        '''
                    }
                }
            }
        }
    }
}
