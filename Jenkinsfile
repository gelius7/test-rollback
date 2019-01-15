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

//@Library("github.com/opsnow-tools/valve-butler")
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
      cluster = input(message:'Select cluster', parameters: [
          [$class: 'ChoiceParameterDefinition', choices: "dev\nstage\nokc1", description: 'test select one', name: 'firstParam']
      ])
      echo ("user input : " + cluster)
      container("builder") {
        butler.env_cluster(cluster)

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