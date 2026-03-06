pipeline {
    agent { label 'pract' }
    parameters {
        choice (
            name: 'ENVIRONMENT', 
            choices: ['dev', 'stage', 'prod'], 
            description: 'Enter Envoronment'
        )
        booleanParam (
            name: 'RUN_TESTS', 
            defaultValue: true, 
            description: 'Run Tests'
        )
        booleanParam (
            name: 'SKIP_DEPLOY', 
            defaultValue: false, 
            description: 'Skip Deploy'
        )
    
    }
    stages {
        stage("Build") {
            steps {
                echo "Building Application"
            }
        }
        stage("Test") {
            when {
                allOf {
                    expression {
                        params.RUN_TESTS
                    }
                    not {
                        branch 'hotfix'
                    }
                }
            }
            steps {
                echo "Running Tests"
            }
        }
        stage("Deploy") {
            when {
                allOf {
                    expression {
                        params.ENVIRONMENT == "stage" &&
                        params.SKIP_DEPLOY == false
                    }
                    not {
                        branch 'develop'
                    }
                }
            }
            steps {
                echo "Deploying to Stage"
            }
        }
        stage("Deploy-Prod") {
            when {
                allOf {
                    expression {
                        params.ENVIRONMENT == "prod" &&
                        params.RUN_TESTS &&
                        params.SKIP_DEPLOY == false
                    }
                    branch 'main'
                }
            }
            steps {
                echo "Deploying to Production"
            }
        }
    }
    
}