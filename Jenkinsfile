pipeline {
  agent any
  parameters {
    string(name: 'SERVICE_REF', defaultValue: 'develop', description: 'Branch/tag/SHA')
  }
  stages {
    stage('Checkout') {
      steps { git branch: params.SERVICE_REF, url: 'https://github.com/PlataformaIntegradaInvestigadores/api-gateway.git' }
    }
    stage('Quality Gate') {
      steps { sh 'docker compose config' }
    }
    stage('Build')       { steps { sh 'docker compose build' } }
    stage('Deploy')      { steps { sh 'docker compose up -d' } }
    stage('Healthcheck') { steps { sh 'curl -f http://localhost:8080/ || exit 1' } }
    stage('Manifest')    { steps { sh 'echo "MANIFEST update: api-gateway SHA=$GIT_COMMIT"' } }
  }
  post { failure { echo 'Deploy failed. Revisar logs.' } }
}
