
node {
  try {
    stage('Preparation') {
      git(
        url: 'https://github.com/juanmabarrio/blog-cicd.git',
        branch: env.BRANCH_NAME
      )
    }
    stage('Create jar') {
      sh 'mvn clean install -B -DskipTests'
    }
    stage('Unit Test') {
      sh 'mvn -Dtest=es.codeurjc.daw.e2e.rest.** -Dmaven.test.failure.ignore test'
    }
    stage('Store jar in CI') {
      archiveArtifacts 'target/*.jar'
    }

    if (env.BRANCH_NAME == 'master') {
      stage("End to end tests"){
         sh 'mvn -Dtest=es.codeurjc.daw.e2e.selenium.** -Dmaven.test.failure.ignore test'
      }
      stage("Quality"){
        sh "mvn sonar:sonar"
      }
      stage('deploy to releases nexus') {
        sh 'mvn deploy -DskipTests'
      }
    }
  }
   finally {
    junit 'target/*-reports/TEST-*.xml'
   }
}
