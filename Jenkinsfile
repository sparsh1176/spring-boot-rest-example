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
                    withAWS(credentials: '41cabaff-889a-40f3-b373-141a258c94d1'){
                        def identity = awsIdentity()
                        sh(script:'''
                        echo ${identity}
                        ''')
                        s3Upload(file:'target/*.war', bucket:'bootcamp-ankur', path:'/')
                    }

                }
            }
        }

    }
}    


