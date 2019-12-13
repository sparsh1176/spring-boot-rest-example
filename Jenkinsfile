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
        stage("s3 upload"){
            steps{
                script{
                    
                        s3Upload(bucket: 'pratyush-bucket', workingDir:'target', includePathPattern:'**/*.war');

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

