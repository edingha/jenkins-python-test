<<<<<<< HEAD
pipeline {
    agent any

    triggers {
        pollSCM('*/5 * * * 1-5')
    }

    options {
        skipDefaultCheckout(true)
        // Keep the 10 most recent builds
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }



    stages {

        stage ("Code pull"){
            steps{
                checkout scm
            }
        }

        stage('Build environment') {
            steps {
                echo "Building virtualenv"
                sh 'virtualenv env -p python3.5'
                sh '. env/bin/activate'
                sh 'env/bin/pip install -r requirements.txt'
            }
        }

        stage('Static code metrics') {
            steps {
                echo "Raw metrics"
                sh  ''' . env/bin/activate
                        radon raw --json irisvmpy > raw_report.json
                        radon cc --json irisvmpy > cc_report.json
                        radon mi --json irisvmpy > mi_report.json
                        sloccount --duplicates --wide irisvmpy > sloccount.sc
                    '''
                echo "Test coverage"
                sh  ''' . env/bin/activate
                        coverage run irisvmpy/iris.py 1 1 2 3
                        python -m coverage xml -o reports/coverage.xml
                    '''
                echo "Style check"
                sh  ''' . env/bin/activate
                        pylint irisvmpy || true
                    '''
            }
            post{
                always{
                    step([$class: 'CoberturaPublisher',
                                   autoUpdateHealth: false,
                                   autoUpdateStability: false,
                                   coberturaReportFile: 'reports/coverage.xml',
                                   failNoReports: false,
                                   failUnhealthy: false,
                                   failUnstable: false,
                                   maxNumberOfBuilds: 10,
                                   onlyStable: false,
                                   sourceEncoding: 'ASCII',
                                   zoomCoverageChart: false])
                }
            }
        }



        stage('Unit tests') {
            steps {
                sh  ''' . env/bin/activate
                        python -m pytest --verbose --junit-xml reports/unit_tests.xml
                    '''
            }
            post {
                always {
                    // Archive unit tests for the future
                    junit allowEmptyResults: true, testResults: 'reports/unit_tests.xml'
                }
            }
        }

        stage('Acceptance tests') {
            steps {
                sh  ''' . env/bin/activate
                        behave -f=formatters.cucumber_json:PrettyCucumberJSONFormatter -o ./reports/acceptance.json || true
                    '''
            }
            post {
                always {
                    cucumber (buildStatus: 'SUCCESS',
                    fileIncludePattern: '**/*.json',
                    jsonReportDirectory: './reports/',
                    parallelTesting: true,
                    sortingMethod: 'ALPHABETICAL')
                }
            }
        }

        stage('Build package') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                sh  ''' . env/bin/activate
                        python setup.py bdist_wheel
                    '''
            }
            post {
                always {
                    // Archive unit tests for the future
                    archiveArtifacts allowEmptyArchive: true, artifacts: 'dist/*whl', fingerprint: true
                }
            }
        }

        // stage("Deploy to PyPI") {
        //     steps {
        //         sh """twine upload dist/*
        //         """
        //     }
        // }
    }


}
=======
pipeline {
    agent any

    triggers {
        pollSCM('*/5 * * * 1-5')
    }

    options {
        skipDefaultCheckout(true)
        // Keep the 10 most recent builds
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }



    stages {

        stage ("Code pull"){
            steps{
                checkout scm
            }
        }

        stage('Build environment') {
            steps {
                echo "Building virtualenv"
                sh 'virtualenv env -p python3.5'
                sh '. env/bin/activate'
                sh 'env/bin/pip install -r requirements.txt'
            }
        }

        stage('Static code metrics') {
            steps {
                echo "Raw metrics"
                sh  ''' . env/bin/activate
                        radon raw --json irisvmpy > raw_report.json
                        radon cc --json irisvmpy > cc_report.json
                        radon mi --json irisvmpy > mi_report.json
                        sloccount --duplicates --wide irisvmpy > sloccount.sc
                    '''
                echo "Test coverage"
                sh  ''' . env/bin/activate
                        coverage run irisvmpy/iris.py 1 1 2 3
                        python -m coverage xml -o reports/coverage.xml
                    '''
                echo "Style check"
                sh  ''' . env/bin/activate
                        pylint irisvmpy || true
                    '''
            }
            post{
                always{
                    step([$class: 'CoberturaPublisher',
                                   autoUpdateHealth: false,
                                   autoUpdateStability: false,
                                   coberturaReportFile: 'reports/coverage.xml',
                                   failNoReports: false,
                                   failUnhealthy: false,
                                   failUnstable: false,
                                   maxNumberOfBuilds: 10,
                                   onlyStable: false,
                                   sourceEncoding: 'ASCII',
                                   zoomCoverageChart: false])
                }
            }
        }



        stage('Unit tests') {
            steps {
                sh  ''' . env/bin/activate
                        python -m pytest --verbose --junit-xml reports/unit_tests.xml
                    '''
            }
            post {
                always {
                    // Archive unit tests for the future
                    junit allowEmptyResults: true, testResults: 'reports/unit_tests.xml'
                }
            }
        }

        stage('Acceptance tests') {
            steps {
                sh  ''' . env/bin/activate
                        behave -f=formatters.cucumber_json:PrettyCucumberJSONFormatter -o ./reports/acceptance.json || true
                    '''
            }
            post {
                always {
                    cucumber (buildStatus: 'SUCCESS',
                    fileIncludePattern: '**/*.json',
                    jsonReportDirectory: './reports/',
                    parallelTesting: true,
                    sortingMethod: 'ALPHABETICAL')
                }
            }
        }

        stage('Build package') {
            when {
                expression {
                    currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                sh  ''' . env/bin/activate
                        python setup.py bdist_wheel
                    '''
            }
            post {
                always {
                    // Archive unit tests for the future
                    archiveArtifacts allowEmptyArchive: true, artifacts: 'dist/*whl', fingerprint: true
                }
            }
        }

        // stage("Deploy to PyPI") {
        //     steps {
        //         sh """twine upload dist/*
        //         """
        //     }
        // }
    }


}
>>>>>>> 6704383a25aef3238e8cbd03837a319ab1b636e4
