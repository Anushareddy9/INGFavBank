node('master') {
    stage('Git SCM Checkout') { 
               git 'https://github.com/Anushareddy9/INGFavBank.git'    
            }
        stage('Java Build')
            {
                 withSonarQubeEnv('sonar')  
                  { 
                  sh '/opt/maven/bin/mvn clean verify sonar:sonar -Dmaven.skip.test=true' 
				  sh '/opt/maven/bin/mvn org.codehaus.mojo:sonar-maven-plugin:2.4:sonar -Dsonar -Dsonar.host.url=http://18.216.14.5:9000 -Dsonar.dynamicAnalysis=true'
                  } 

            }
        stage('Quality Gate')
            {
                timeout(time:1,unit:'MINUTES')
                    {
                    def qg=waitForQualityGate()
                    if(qg.status != 'OK')
                      {
                        error "Pipeline aborted due to quality gate failure:${qg.status}"
                      }
                    }
            }
        stage('Deploy') { 
              sh '/opt/maven/bin/mvn clean deploy '
            }
        stage('Release')
            {
               sh 'export JENKINS_NODE_COOKIE=dontkillme ;nohup java -jar $WORKSPACE/target/*.jar &'
            }
  }
