pipeline {
    agent {
        label {
            label 'slave-1'
            customWorkspace '/mnt/war'
        }
    }
    stages {
        stage ('SCM') {
            steps {
                sh 'rm -rf *'
                sh 'git clone https://github.com/SShifa/vel-app.git'
            }
        }
        stage ('BUILD') {
            steps {
                 sh '''
                cd /mnt/war/new-project/gameoflife-java21
                /mnt/build-tool/apache-maven-3.9.16/bin/mvn clean install
                '''
            }
        }
        stage ('UPLOAD-WAR-TO-S3') {
            steps {
                 sh '''
                aws s3 cp /mnt/war/new-project/vel-app/java21/target/vel-app.war s3://war-gameoflife-shifa/
                '''
            }
        }
        stage ('DEPLOY-ON-SLAVE') {
            agent {
                label {
                    label 'slave-2'
                }
            }
            steps {
                 sh '''
                aws s3 cp s3://war-gameoflife-shifa/vel-app.war /mnt/apache-tomcat-10.1.55/webapps/
                '''
            }
        }
    }
}
