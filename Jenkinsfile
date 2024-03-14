pipeline{
    
    agent any
    tools {
        jdk 'JAVA_HOME'
        maven 'MAVEN_HOME'
    }
    environment {
        PATH = "$PATH:C:/maven/apache-maven-3.9.6-bin (3)/apache-maven-3.9.6/bin"
    }
    stages{
  
       stage('Code Compile'){
            steps{
                 bat 'mvn clean compile'
                
            }
         }
         
         
       stage('Unit test'){
            steps{
                 bat 'mvn test'
                
            }
         }
         
        stage('Integration test'){
            steps{
                 bat 'mvn verify -DskipUnitTests'
                
            }
         }
         
         stage('Maven build'){
            steps{
                 bat 'mvn clean install'
                
            }
         }
        
         stage('OWASP Dependency Check'){
            steps{
                 dependencyCheck additionalArguments: '', odcInstallation: 'DP'
                
            }
         } 
         
       stage('Testing by sonar'){
            steps{
                
                 bat ''' mvn sonar:sonar -Dsonar.url-http://localhost:9000/ -Dsonar.login-squ_abb67d36969df874cb80b76166032e71de78defa -Dsonar.projectname-petclinic \
                     -Dsonar.java.binaries=. \
                     -Dsonar.projectKey=petclinic '''
            
            }
         }   
         
        stage('Clean Package'){
            steps{
                 bat 'mvn clean package'
             }
            post{
                success{
                    echo"Archiving Artifact"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }  
         } 
        
        stage('Tomcat'){
            steps{
               deploy adapters: [tomcat9(credentialsId: 'e24b8dc2-46c5-4d59-bde4-e21a91a33bbd', path: '', url: 'http://localhost:8081/')], contextPath: 'Petclinic', war: '**/*.war'
    
               }
                
            }
    }
    post{
        
          success{
            
             emailext attachLog: true, body: '''Your Petclinic pipeline is successfully completed.<br>
For check pipeline result click the blog.<br><br>

Thanks.<br>
-DevOps Team Ximple Solutions''', subject: 'Petclinic pipeline result', to: 'ompanchalait@gmail.com'
           
            }
        failure{
            emailext attachLog: true, body: '''Your Petclinic pipeline is unfortunately failed.<br>
For check pipeline result click the blog.<br><br>

Thanks.<br>
-DevOps Team Ximple Solutions''', subject: 'Petclinic pipeline result', to: 'ompanchalait@gmail.com'
           
        }
    }
}



