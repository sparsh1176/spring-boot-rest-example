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
                    withMaven (maven: "mavenTool")
                    {
                    sh(script:"""
                    mvn clean package
                    """)
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
                    withAWS(region:'us-east-1',credentials:'aws_cred'){
                        def identity = awsIdentity()
                        sh(script:'''
                        aws autoscaling set-desired-capacity --auto-scaling-group-name ankur_ASG --desired-capacity 4 --honor-cooldown
                        sleep 200s
                        aws autoscaling set-desired-capacity --auto-scaling-group-name ankur_ASG --desired-capacity 2 --honor-cooldown
                        ''')

                    }
                }
            }
        }
        }
}    


