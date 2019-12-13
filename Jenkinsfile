pipeline {
    
    agent any
    
    stages{
        stage("git pull"){
            steps{
                script{
                    git branch: "master", changelog: false, url: "https://github.com/pratyushgarg15/spring-boot-rest-example.git" 
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
        stage("Upload to S3"){
            steps{
                script{
                    sh(script:'''
                    aws s3 cp /var/lib/jenkins/workspace/spring-boot/target/*.war s3://sparsh117612/
                    ''')
                    }
                }
            }
          stage("Downladed to S3"){
            steps{
                script{
                    sh(script:'''
                     aws s3 cp s3://sparsh117612/spring-boot-rest-example-0.5.0.war /var/lib
                    ''')
                    }
                }
            }




    
    
    
    }
}
