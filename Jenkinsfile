node('php') {
    stage 'Checkout Projet'
        git branch: 'master', credentialsId: 'jenkins-private-key', url: 'r7g5a3iruep6y@git.eu.magento.cloud:r7g5a3iruep6y.git'

    stage 'Composer Install'
        sh 'composer install --prefer-dist --no-interaction --ignore-platform-reqs'
        sh 'composer dumpautoload --no-interaction'

    stage 'Analyse Code SpBuilder'
        sh './bin/spbuilder analyze --ignore-tool visualization'

    // stage 'Analyse Code SmileAnalyser'
    //    sh './bin/SmileAnalyser jenkinsbuild --api_user lamin --api_token xxxxxxxxxxxxxxxxxxxxxxxxxxxxx --api_url ${JOB_URL} --build ${BUILD_NUMBER}'

    stage 'Publish Results'
        step([$class: 'CheckStylePublisher', canComputeNew: false, defaultEncoding: '', healthy: '', pattern: 'build/logs/checkstyle.xml', unHealthy: ''])
        step([$class: 'PmdPublisher', canComputeNew: false, defaultEncoding: '', healthy: '', pattern: 'build/logs/pmd.xml', unHealthy: ''])
        step([$class: 'DryPublisher', canComputeNew: false, defaultEncoding: '', healthy: '', pattern: 'build/logs/cpd.xml', unHealthy: ''])
        step([
            $class: 'ViolationsPublisher',
            violationConfigs: [
                [ pattern: 'build/logs/checkstyle.xml', reporter: 'CHECKSTYLE' ],
                [ pattern: 'build/logs/pmd.xml', reporter: 'PMD' ],
                [ pattern: 'build/logs/cpd.xml', reporter: 'CPD' ]
            ]
        ])
}