dslPodName = "contraDsl-${UUID.randomUUID()}"
dockerRepoURL = '172.30.254.79:5000'
openshiftNamespace = 'ember-csi'
openshiftServiceAccount = 'jenkins'

createDslContainers podName: dslPodName,
                    dockerRepoURL: dockerRepoURL,
                    openshiftNamespace: openshiftNamespace,
                    openshiftServiceAccount: openshiftServiceAccount,
{
  node(dslPodName){
    stage("pre-flight"){
      deleteDir()
      checkout scm
    }

    stage("Parse Configuration"){
      parseConfig()
      echo env.configJSON
    }

    stage("Execute Tests"){
      try {
        executeTests verbose: true,
                     vars: [ workspace: "${WORKSPACE}" ],
                     ansibleContainerName: "ansible-executor"
      } finally {
        junit 'junit.xml'
      }
    }
  }
}
