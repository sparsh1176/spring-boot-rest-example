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
          stage("intance"){
            steps{
                script{
                    sh(script:'''
                    aws aws configure set default.region us-east-1
                    ''')
                    }
                }
            }
         stage("intance"){
            steps{
                script{
                    sh(script:'''
                    aws ec2 run-instances --image-id ami-037a66cee192b7786 --count 1 --instance-type t2.micro --key-name Sparsh11761 --security-group-ids sg-0dfa1c170a62c3cf9 --subnet-id subnet-8599a8ab
                    ''')
                    }
                }
            }
    }
}

