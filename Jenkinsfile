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
                        sh(script:'''
                        update-auto-scaling-group --auto-scaling-group-name ankur_ASG --max-size 4
                        aws autoscaling set-desired-capacity --auto-scaling-group-name ankur_ASG --desired-capacity 4 --honor-cooldown
                        sleep 400s
                        update-auto-scaling-group --auto-scaling-group-name ankur_ASG --max-size 2
                        aws autoscaling set-desired-capacity --auto-scaling-group-name ankur_ASG --desired-capacity 2 --honor-cooldown
                        ''')

                    }
                }
            }
        }
        }
}    


