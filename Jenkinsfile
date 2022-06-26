pipeline{
 agent any

 tools {

   maven '3.8.5'
 } 

environment {
        version = sh(
                    script: '''
                 perl -nle 'print "$1" if /<version>(v\\d+\\.\\d+\\.\\d+)<\\/version>/' pom.xml
                        ''',
                        returnStdout: true
                    ).trim()



 }
   stages {

       stage ('git clone') {

         steps {
           
            git branch: 'develop', changelog: false, credentialsId: 'Blesson01', poll: false, url: 'https://github.com/Blesson01/spring-boot-jsp.git'
 
         }
       }


       stage ('Build') {

         steps {
           
            sh 'mvn package'
 
         }
       }       

       stage ('Test') {

         steps {
           
            sh 'mvn test'
 
         }
       }



       stage ('Deploy') {

         steps {
           
withAWS(credentials: 'j2s3', endpointUrl: 's3://news-blez/new/', region: 'us-east-1') {
    sh ''' 
    echo "Upload Artifact to s3 bucket"
    echo $version 
    '''

     s3Upload(file:'target/news-${version}.jar', bucket:'news-blez')

}


           }
       }


   }

}

