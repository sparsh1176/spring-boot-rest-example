pipeline {
    
    agent any
    
    stages{
        stage("git pull"){
            steps{
                script{
                    git branch: "master", changelog: false, url: "https://github.com/ankurbhatt04/spring-boot-rest-example" 
                }   
            }
        }
        stage("maven"){
            steps{
                script{
                    withMaven (maven: "mavenTool")
                    {
                    sh(script:"""
                    apt-get install -y openjdk-8-jdk
                    mvn clean package
                    """)
                    }
                }
            }
        }
        // stage("Upload to S3"){
        //     steps{
        //         script{

        //         }
        //     }
        // }

    }
}    


