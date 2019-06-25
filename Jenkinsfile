node ('dockerhost') {
    deleteDir()
    stage('repo checkout') {
        git (url: "git@github.com:mfominov/ci-cd-examples-js-sonarqube.git",
            credentialsId: 'id_rsa_jenkins_gitlab',
            branch: 'master'
        )
    }
    stage('npm publish') {
        env.NPM_EMAIL="lm-sa-devops@leroy-merlin.ru"
        docker.image('docker.art.lmru.tech/node:10-stretch').inside() { c ->
            withNPM(npmrcConfig: 'devops_npmrc_art') {
                withCredentials([string(credentialsId: 'sonarqube_token', variable: 'sonarqube_token')]) {
                    sh """npm ci
                          echo -e '\nsonar.login='\$(echo -n '$sonarqube_token') >> ./sonar-project.properties
                          npm run sonar"""
                }
            }
        }
    }
}
