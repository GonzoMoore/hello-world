node ('test-1')
{
    stage('Setup') {
        // Let's assume we are testing the development branch of
        // all repositories.
        def api_branch = 'development'
        def ui_branch = 'development'

        // We need to determine which repo triggered this build
        // and for that repo we'll use the triggering branch.
        if (env.CHANGE_URL.contains("gonzomoore/hello-world")) {
            def api_branch = env.BRANCH_NAME
        } else if (env.CHANGE_URL.contains("gonzomoore/docker-compose-demo")) {
            def ui_branch = env.BRANCH_NAME
        }
    }

    stage('Checkout Code') {

        // User a temporary directory to store repo contents.
        dir("/tmp/$BUILD_TAG") {

            // Checkout main repository to workspace.
            checkout([
                $class: 'GitSCM',
                branches: [[name: ${api_branch}]],
                doGenerateSubmoduleConfigurations: false,
                extensions: [
                    [$class: 'LocalBranch', localBranch: '**'],
                    [$class: 'RelativeTargetDirectory', relativeTargetDir: 'api']
                ],
                submoduleCfg: [],
                userRemoteConfigs: [[credentialsId: 'github-gonzomoore', url: 'https://github.com/GonzoMoore/hello-world']]
            ])

            // Checkout ui repository to 'ui' sub-directory.
            checkout([
                $class: 'GitSCM',
                branches: [[name: ${ui_branch}]],
                doGenerateSubmoduleConfigurations: false,
                extensions: [
                    [$class: 'LocalBranch', localBranch: '**'],
                    [$class: 'RelativeTargetDirectory', relativeTargetDir: 'ui']
                ],
                submoduleCfg: [],
                userRemoteConfigs: [[credentialsId: 'github-gonzomoore', url: 'https://github.com/GonzoMoore/docker-compose-demo']]
            ])
            
        }
        
    }
}
