def SERVICE_GROUP = "svc-grp"
def SERVICE_NAME = "svc-name"
def IMAGE_NAME = "${SERVICE_GROUP}-${SERVICE_NAME}"
def REPOSITORY_URL = "https://github.com/gelius7/test-spring-boot.git"
def REPOSITORY_SECRET = ""
def SLACK_TOKEN_DEV = ""
def SLACK_TOKEN_DQA = ""

def cluster = "cc"
def namespace = "nn"
def revision = "rr"

@Library("github.com/gelius7/valve-butler")
def butler = new com.opsnow.valve.v8.Butler()
def label = "worker-${UUID.randomUUID().toString()}"

properties([
  buildDiscarder(logRotator(daysToKeepStr: "60", numToKeepStr: "30"))
])
podTemplate(label: label, containers: [
  containerTemplate(name: "builder", image: "quay.io/opsnow-tools/valve-builder", command: "cat", ttyEnabled: true, alwaysPullImage: true)
], volumes: [
  // hostPathVolume(mountPath: "/var/run/docker.sock", hostPath: "/var/run/docker.sock"),
  // hostPathVolume(mountPath: "/home/jenkins/.draft", hostPath: "/home/jenkins/.draft"),
  // hostPathVolume(mountPath: "/home/jenkins/.helm", hostPath: "/home/jenkins/.helm")
]) {
  node(label) {
    stage("Input Parameters") {
      container("builder") {
        echo "Select cluster"
        param = input(message:'Input Parameters ', parameters: [
            [$class: 'ChoiceParameterDefinition', choices: "dev\nstage\nokc1", description: 'Select cluster', name: 'sel_cluster']
        ])

        cluster = param
        def nmspace_list = butler.scan_helm(cluster)
        echo nmspace_list

        echo "Input your namespace"
        param = input(message:'Input Parameters ', parameters: [
            [$class: 'ChoiceParameterDefinition', choices: nmspace_list, description: 'Select cluster', name: 'sel_cluster']
//            [$class: 'TextParameterDefinition', defaultValue: ${namespace}, description: 'Select Namespace', name: 'sel_namespace']
        ])
        namespace = param
        def list = butler.scan_helm_namespace(namespace)

        echo ("user input : " + cluster)
        echo ("user input : " + namespace)
        echo list
      }
    }
    stage("Rollback") {
      container("builder") {
        // cluster, namespace, revision
//        butler.rollback()
        echo cluster
        echo namespace
        echo revision
      }
    } 
  }
}
