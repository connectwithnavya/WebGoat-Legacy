pipeline {
Â Â Â Â agent any

Â Â Â Â tools {
Â Â Â Â Â Â Â Â maven 'mvn3916'
Â Â Â Â Â Â Â Â jdk 'jdk8'
Â Â Â Â }

Â Â Â Â environment {
Â Â Â Â Â Â Â Â ARTEFACT_NAME = "${WORKSPACE}/target/WebGoat-${BUILD_VERSION}.war"
Â Â Â Â Â Â Â Â IQ_SCAN_URL = ""
Â Â Â Â }

Â Â Â Â stages {
Â Â Â Â Â Â Â Â stage('Build') {
Â Â Â Â Â Â Â Â Â Â Â Â steps {
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â sh 'mvn -B -Dproject.version=$BUILD_VERSION -Dmaven.test.failure.ignore clean package'
Â Â Â Â Â Â Â Â Â Â Â Â }
Â Â Â Â Â Â Â Â Â Â Â Â post {
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â success {
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â echo 'Now archiving...'
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â archiveArtifacts artifacts: "**/target/*.war"
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â }
Â Â Â Â Â Â Â Â Â Â Â Â }
Â Â Â Â Â Â Â Â }
Â Â Â Â Â Â Â Â stage('Nexus IQ Scan'){
Â Â Â Â Â Â Â Â Â Â Â Â steps {
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â script{

Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â def policyEvaluation = nexusPolicyEvaluation (
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â advancedProperties: '',
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â enableDebugLogging: false,
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â failBuildOnNetworkError: false,
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â failBuildOnScanningErrors: false,
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â iqApplication: selectedApplication('webgoat'),
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â iqInstanceId: 'nxiq',
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â iqScanPatterns: [[scanPattern: '**/*.war']],
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â iqStage: 'build',
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â jobCredentialsId: 'sonatype',
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â reachability: [
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â javaAnalysis: [
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â enable: true
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â ]
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â ]
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â )

Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â echo "Nexus IQ scan succeeded: ${policyEvaluation.applicationCompositionReportUrl}"
Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â IQ_SCAN_URL = "${policyEvaluation.applicationCompositionReportUrl}"

Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â Â }
Â Â Â Â Â Â Â Â Â Â Â Â }
Â Â Â Â Â Â Â Â }

Â Â Â Â }
}
