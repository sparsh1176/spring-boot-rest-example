pipeline {
    
    agent any
    stages{
        stage("git pull"){
            steps{
                script{
                    git branch: "master", changelog: false, url: "https://github.com/khoubyari/spring-boot-rest-example.git" 
                }   
            }
        }
        stage("maven"){
            steps{
                script{
                    withMaven (maven: "mavenTool")
                    {
                    sh(script:"""
                             mvn clean package
                    """)
                    }
                }
            }
        }
        stage("upload s3"){
            steps{
                script{
                    sh(script:"""
                    aws s3 cp /var/lib/jenkins/workspace/Sparsh-pipe/target/spring-boot-rest-example-0.5.0.war s3://sparsh-s3
                    """)

                }

            }
        }
    }
}    


