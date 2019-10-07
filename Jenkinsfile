pipeline {
    
    agent any
    stages{
        stage("git pull"){
            steps{
                script{
                    git branch: "master", changelog: false, url: "https://github.com/khoubyari/spring-boot-rest-example.git" 
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
        stage("Push to S3"){
            steps{
                script{
                        {
                        sh(script:"""
                                aws s3 cp /var/lib/jenkins/workspace/Sparsh-pipe/target/spring-boot-rest-example-0.5.0.war s3://sparsh-s3
                            """)
                        }    
                    }            
                }
            }
        stage("Making a new instance"){
            steps{
                script{
                    {
                        sh(script:"""
                                aws ec2 run-instances   --image-id ami-00baf89e57474f82f --key-name Sparsh --security-groups EC2SecurityGroup --instance-type t2.micro --placement AvailabilityZone=us-east-1a --block-device-mappings DeviceName=/dev/sdh,Ebs={VolumeSize=10} --count 1
                            """)
                }
            }
        }
    }
}
}    


