## What is the image for?
The intended purpose of this image is for it to be used as a Jenkins agent. By using the installed features the user is able to create Jenkins pipelines that can trigger [Skaffold](https://skaffold.dev/) commands to run a `skaffold.yaml` file. We can also implement the [Container Structure Test](https://github.com/GoogleContainerTools/container-structure-test) for static testing of the image. An example of using this image as a Jenkins agent via [Kubernetes](https://plugins.jenkins.io/kubernetes/) can be seen below. 

First, an example of configuring the pod template in yaml to create the agent.

```yaml
jenkins:
  clouds:
    - kubernetes:
        name: "kubernetes"
        templates:
          - name: "image-builder-skaffold"
            label: "image-builder-skaffold"
            nodeUsageMode: NORMAL
            containers:
              - name: "image-skaffold"
                image: "ghcr.io/liatrio/image-builder-skaffold:${builder_images_version}"
```
And then specifying the agent in the Jenkinsfile for an example step.

```jenkins
stage('Build') {
  agent {
    label "image-builder-terraform"
  }
  steps {
    container('image-skaffold') {
      sh "skaffold build"
    }
  }
}
```

> **_NOTE:_** For more information on using Skaffold in a Jenkins pipeline, read the [Liatrio's Blog Post](https://www.liatrio.com/blog/delivery-pipelines-with-skaffold) on the topic.

## What is installed on this image?
- Version [1.37.X](https://github.com/GoogleCloudPlatform/skaffold/releases/download/v1.37.2/skaffold-linux-amd64) of Skaffold, a command line tool that facilitates continuous development for Kubernetes applications
- Version [1.24](https://github.com/aws/aws-cli) of the AWS CLI
- Version [1.11.X](https://storage.googleapis.com/container-structure-test/v1.11.0/container-structure-test-linux-amd64) of Container Structure Tests, a framework to validate the structure of a container image
- Version [1.24.X](https://storage.googleapis.com/kubernetes-release/release/v1.24.0/bin/linux/amd64/kubectl) of kubectl, a command-line tool that allows you to run commands against Kubernetes clusters
- Version [3.9.0](https://get.helm.sh/helm-v3.9.0-linux-amd64.tar.gz) of Helm, a package manager for Kubernetes

> **_NOTE:_** For more information on using Skaffold in a Jenkins pipeline, read [Liatrio's Blog](https://www.liatrio.com/blog/delivery-pipelines-with-skaffold) on the topic.
