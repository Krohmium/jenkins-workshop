
/* 
    This is the Pretested Integration Jenkinsfile
    It enables the pretested workflow for this repository, and will automatically be picked up by Jenkins
*/
node {
    stage("checkout") {
        //Using the Pretested integration plugin to checkout out any branch in the ready namespace
        checkout(
            [$class: 'GitSCM', 
            branches: [[name: '*/ready/**']], 
            doGenerateSubmoduleConfigurations: false, 
            extensions: [[$class: 'CleanBeforeCheckout'], 
            pretestedIntegration(gitIntegrationStrategy: squash(), 
            integrationBranch: 'master', 
            repoName: 'origin')], 
            submoduleCfg: [], 
            userRemoteConfigs: [[credentialsId: 'krohmium-1', //remember to change credentials and url.
            url: 'git@github.com:Krohmium/jenkins-workshop.git']]])
    }
    stage("clean install"){
        // run maven tests here
	sh 'docker run -i -u $(id -u):$(id -g) -v $PWD:/usr/src/mymaven -w /usr/src/mymaven --rm maven:3-jdk-8 mvn clean install'
    }
    stage("artivvkts"){
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
    }
    stage("publish"){
        //This publishes the commit if the tests have run without errors
        pretestedIntegrationPublisher()
	
    }
}
