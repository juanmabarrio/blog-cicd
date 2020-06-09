
node {
  try {
    stage('Preparation') {
      git(
        url: 'https://github.com/juanmabarrio/blog-cicd.git',
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

    if (env.BRANCH_NAME == 'develop') {
      stage('deploy to snapshot nexus') {
        sh 'mvn deploy '
      }
    } else if (env.BRANCH_NAME == 'master') {
      stage('deploy to releases nexus') {
        sh 'mvn deploy'
      }
    }
  }
   finally {
    junit 'target/*-reports/TEST-*.xml'
   }
}
