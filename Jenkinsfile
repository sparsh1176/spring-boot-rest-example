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
                    aws s3 cp /var/lib/jenkins/workspace/Sparsh/target/*.war s3://sparsh-s3/
                    ''')
                    }
                }
            }
        stage("Set Desired Capacity in ASG"){
            steps{
                script{
                    withAWS(region:'us-east-1',credentials:'aws_bootcamp'){
                        def identity = awsIdentity()
                        sh(script:'''
                        aws autoscaling update-auto-scaling-group --auto-scaling-group-name Sparsh --max-size 2
                        aws autoscaling set-desired-capacity --auto-scaling-group-name Sparsh --desired-capacity 1 --honor-cooldown
                        sleep 200s
                        aws autoscaling set-desired-capacity --auto-scaling-group-name Sparsh --desired-capacity 1 --honor-cooldown
                        aws autoscaling update-auto-scaling-group --auto-scaling-group-name Sparsh --max-size 1
                        ''')

                    }
                }
            }
        }
        }
}    

