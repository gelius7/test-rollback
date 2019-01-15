def cluster = ""

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
    stage("Select cluster") {
      container("builder") {
        echo "Select cluster" 
        param = input(message:'Select cluster ', parameters: [
            [$class: 'ChoiceParameterDefinition', choices: "dev\nstage\nokc1", description: 'Select cluster', name: 'sel_cluster']
        ])
        cluster = param
      }
    }
    stage("Select Image") {
      container("builder") {
        def list = butler.scan_helm(cluster)

        echo "Select your Image"
        param = input(message:'Select Image ', parameters: [
            [$class: 'ChoiceParameterDefinition', choices: list, description: 'Select cluster', name: 'sel_image']
        ])

        image = param
      }
    }
    stage("Rollback") {
      container("builder") {
        sh """
          helm rollback ${image} 0
          helm history ${image}
        """
      }
    } 
  }
}
