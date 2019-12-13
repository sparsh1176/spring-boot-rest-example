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

