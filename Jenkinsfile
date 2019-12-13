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
         stage("Increase ASG desired capacity"){
            steps{
                withAWS(region:'us-east-1'){
                    sh ''' aws autoscaling set-desired-capacity --auto-scaling-group-name pratyush-ASG --desired-capacity 2
 '''            }
                }
            }
        
        stage("Wait for Deployment"){
            steps{
                script{
                    sleep(300);
                }
            }
        }
        stage("Decrease ASG desired capacity"){
            steps{
                withAWS(region:'us-east-1'){
                    sh ''' aws autoscaling set-desired-capacity --auto-scaling-group-name pratyush-ASG --desired-capacity 1
'''
                }
            }
        }

    }
}

