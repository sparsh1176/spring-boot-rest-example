pipeline {
    
    agent any
    
    stages{
        stage("git pull"){
            steps{
                script{
                    git branch: "master", changelog: false, url: "https://github.com/ankurbhatt04/spring-boot-rest-example/blob/master/Jenkinsfile" 
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
    }
}    


