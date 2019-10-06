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
                    withAWS(credentials:"aws_access_keys"){
                        def identity = awsIdentity()
                        sh(script:'''
                        echo ${identity}
                        ''')
                        s3Upload(file:'*.war', bucket:'bootcamp-ankur', path:'/var/lib/jenkins/workspace/ankur-test/target/*.war')
                    }

                }
            }
        }

    }
}    


