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
                    aws s3 cp /var/lib/jenkins/workspace/ankur-test/target/*.war s3://bootcamp-ankur/
                    ''')
                    }
                }
            }
        stage("Set Desired Capacity in ASG"){
            steps{
                script{
                    withAWS(region:'us-east-1',credentials:'aws_bootcamp'){
                        def identity = awsIdentity()
                    }
                }
            }
        }
        }
}    


