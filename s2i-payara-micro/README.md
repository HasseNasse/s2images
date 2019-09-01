# S2I for Payara Micro

S2I for Payara Micro Java EE containers. Build on top of CentOS 7 with high configurability.  
https://github.com/HasseNasse/s2images/tree/master/s2i-payara-micro
https://hub.docker.com/r/hassenasse/s2i-payara-micro

| Java Version | JVM                                   |
| ------------ | ------------------------------------- |
| 11.0.4       | HotSpot OpenJDK 64-Bit Server VM 18.9 |
| 8            | HotSpot OpenJDK 64-Bit Server VM      |

| Application Server | Application Server Version | Java Version | Image Tag   |
| ------------------ | -------------------------- | ------------ | ----------- |
| Payara Micro       | 5.193                      | 11.0.4       | 5.193-jdk11 |
| Payara Micro       | 5.193                      | 8            | 5.193-jdk8  |

## Introduction

Source-to-Image (S2I) is a toolkit and workflow for building reproducible container images from source code. S2I produces ready-to-run images by injecting source code into a container image and letting the container prepare that source code for execution. By creating self-assembling builder images, you can version and control your build environments exactly like you use container images to version your runtime environments. (Source: https://github.com/openshift/source-to-image)

## Usage

This image allows providing application source and building a runnable image, which can be deployed to your docker-engine or Kubernetes/OpenShift cluster. The application source should be packaged as a ThinWar (Source Code + Internal Dependencies).

For an example project, see: https://github.com/HasseNasse/jakartaee-mp-archetype

### 1. Local Environment

Use the s2i CLI to create images using this builder image. You can download the CLI from this source: https://github.com/openshift/source-to-image/releases

Run the following command:  
`s2i build <APP_DIR> hassenasse/s2i-payara-micro:<LIBERTY_VERSION> <APP_NAME> --copy`

_Example:_  
`s2i build ~/dev/FruitApp hassenasse/s2i-payara-micro:5.193-jdk11 fruit-app --copy`

This command will create a local imagestream which can be inspected using `docker image ls`, this imagestream will have the name `<APP_NAME>`.

After a successfull build, the container can be initiated using the following command:  
`docker container run -d -p 9080:9080 -p 9443:9443 --name <APP_NAME> <APP_NAME>:latest`

### 2. OpenShift Deployment

Retrieve the image from hub.docker.com to your local registry:  
`docker pull hassenasse/s2i-payara-micro:5.193-jdk11`

Import the s2i image to your OpenShift / MiniShift image registry:  
`oc import-image s2i-payara-micro --from=hassenasse/s2i-payara-micro:5.193-jdk11 --confirm`

Create a new openshift project:  
`oc new-project <PROJECT_NAME>`

#### Binary Builds

After importing the image to our cluster registry (see 'OpenShift Deployment'), we can choose to build our applications directly from source.

Create an application using the s2i image:  
`oc new-build --name=<APP_NAME> --image-stream=<PROJECT_NAME>/s2i-payara-micro:5.193-jdk11 --binary`

Start a binary build, pointing to the `<APP_DIR>`:  
`oc start-build bc/<APP_NAME> --from-dir <APP_DIR> --follow`

After the image is build and pushed to your image registry, we can view our newly built s2i using:  
`oc get is --selector="app=<APP_NAME>"`  
We should be able to see an image in the format:

| NAME       | DOCKER REPO                   | TAGS   | UPDATED              |
| ---------- | ----------------------------- | ------ | -------------------- |
| <APP_NAME> | .../<PROJECT_NAME>/<APP_NAME> | latest | a few seconds ago... |

We can now deploy our pre-packaged image:  
`oc new-app <PROJECT_NAME>/<APP_NAME>:latest --name <APP_NAME>`

## Configurability

...
