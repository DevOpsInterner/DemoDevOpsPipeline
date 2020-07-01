pipeline {
  agent any
  tools {
    maven 'maven'
  }

  stages {
    stage ('Initialize') {
      steps {
         sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
        echo "Starting Your Pipeline Build"
      }
    }

    stage ('Build') {
      steps {
        sh 'mvn clean package'
        //echo " Build always works Lol"
       }
    }

    stage("Artifact Upload"){
     // echo "Add steps"  [[classifier: '', extension: '', filePath: 'target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar']]
      steps{
       nexusPublisher nexusInstanceId: 'yacine', nexusRepositoryId: 'devopsdemo',[[classifier: '', extension: '', filePath: 'target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar']], packages: [[$class: 'MavenPackage', mavenAssetList: [], mavenCoordinate: [artifactId: 'spring-petclinic', groupId: 'org.springframework.samples', packaging: 'jar', version: '2.3']]]
      }
    }

   stage ('Deploy Using Ansible') {
      steps {
        sh 'cp target/*.jar $ANSIBLE_DIRECTORY/dist/'
        sshagent(['610d3050-5b62-4edc-8395-acddb916ec5c']) {
            sh '''
              cd $ANSIBLE_DIRECTORY
              ansible-playbook -i inventory deploywebapp.yaml
            '''
          }
       }
    }

  }

}
