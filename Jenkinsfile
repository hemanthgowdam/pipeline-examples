#Declarativepipeline
pipeline {
  agent any
  stages {
    stage(SCMGithub) {
      steps {
        checkout scm: [
          $class: 'GitSCM',
          branches: [
            [name: '*/master']
          ],
          userRemoteConfigs: [
            [url: 'https://github.com/hemanthgowdam/pipeline-examples.git']
          ]
        ], lightweight: true

      }
    }
    stage(Build) {
      steps {
        sh 'mvn build'
      }
    }
    stage(Test) {
      steps {
        sh 'mvn test'
      }
    }
    stage(Posttest) {
      steps(success) {
        echo "Pipeline is completed"
      }
      steps(failure) {
        echo "pipeline is failed"
      }
    }
  }
}
