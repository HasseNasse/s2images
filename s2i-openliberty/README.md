# S2I for IBM OpenLiberty

S2I for IBM Openliberty Java EE containers. Build on top of CentOS 7 with high configurability.

| Java Version | JVM                                   |
|--------------|---------------------------------------|
| 11.0.4       | HotSpot OpenJDK 64-Bit Server VM 18.9 |

| Application Server | Application Server Version | Image Tag         |
|--------------------|----------------------------|-------------------|
| IBM OpenLiberty    | 19.0.0.8                   | 19.0.0.8 / latest |

## Introduction

Source-to-Image (S2I) is a toolkit and workflow for building reproducible container images from source code. S2I produces ready-to-run images by injecting source code into a container image and letting the container prepare that source code for execution. By creating self-assembling builder images, you can version and control your build environments exactly like you use container images to version your runtime environments. (Source: https://github.com/openshift/source-to-image)

## Usage

This image allows providing application source and building a runnable image, which can be deployed to your docker-engine or Kubernetes/OpenShift cluster. The application source should be packaged as a ThinWar (Source Code + Internal Dependencies).

For an example project, see: https://github.com/HasseNasse/jakartaee-mp-archetype

### 1. Local Environment

Use the s2i CLI to create images using this builder image. You can download the CLI from this source: https://github.com/openshift/source-to-image/releases

Run the following command:  
`s2i build <APP_DIR> hassenasse/s2i-openliberty:<LIBERTY_VERSION> <APP_NAME> --copy`

_Example:_  
`s2i build ~/dev/FruitApp hassenasse/s2i-openliberty:19.0.0.8 fruit-app --copy`

This command will create a local imagestream which can be inspected using `docker image ls`, this imagestream will have the name `<APP_NAME>`.

After a successfull build, the container can be initiated using the following command:  
`docker container run -d -p 9080:9080 -p 9443:9443 --name <APP_NAME> <APP_NAME>:latest`

### 2. OpenShift Deployment

Retrieve the image from hub.docker.com to your local registry:  
`docker pull hassenasse/s2i-openliberty:latest`

Import the s2i image to your OpenShift / MiniShift image registry:  
`oc import-image s2i-openliberty --from=hassenasse/s2i-openliberty:latest --confirm`

Create a new openshift project:  
`oc new-project <PROJECT_NAME>`

#### 2.1 Binary Builds

After importing the image to our cluster registry (see 'OpenShift Deployment'), we can choose to build our applications directly from source.

Create an application using the s2i image:  
`oc new-build --name=<APP_NAME> --image-stream=<PROJECT_NAME>/s2i-openliberty --binary`

Start a binary build, pointing to the `<APP_DIR>`:  
`oc start-build bc/<APP_NAME> --from-dir <APP_DIR> --follow`

After the image is build and pushed to your image registry, we can view our newly built s2i using:  
`oc get is --selector="app=<APP_NAME>"`  
We should be able to see an image in the format:  
  
| NAME       | DOCKER REPO                   | TAGS   | UPDATED              |
|------------|-------------------------------|--------|----------------------|
| <APP_NAME> | .../<PROJECT_NAME>/<APP_NAME> | latest | a few seconds ago... |

We can now deploy our pre-packaged image:  
`oc new-app <PROJECT_NAME>/<APP_NAME>:latest --name <APP_NAME>`

#### 2.2 Source Code Builds

Alternatively, after importing the image to our cluster registry (see 'OpenShift Deployment'), we can choose to build our applications directly from source.

We can also create source builds by directly pointing to an existing github repository:  
`oc new-build <PROJECT_NAME>/s2i-openliberty~https://github.com/<GH_USER>/<APP_REPO>.git`

Then after a successfull build, we declare a new app to trigger a deploy:  
`oc new-app test-project/test-app-liberty:latest --name test-app-liberty`

## Configurability

All liberty server configurations should be added to the `<APP_DIR>/liberty` folder. These files can include the following:

| File/Folder Name     |                                                                                                                                                        Description                                                                                                                                                        |
| -------------------- | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| server.xml           |                                                                                                      The application server configuration is described in a series of elements in the server.xml configuration file.                                                                                                      |
| server.env           |                                                                                                 The Liberty specific environment variables can be configured in the server.env file to customize the Liberty environment.                                                                                                 |
| jvm.options          |                                                                          The jvm.options files are optional but, when present, are read by the bin/server shell script to determine what options to use when launching the JVM for Open Liberty.                                                                          |
| bootstrap.properties | The bootstrap.properties file is optional but, when present, it is read during Open Liberty bootstrap to provide config for the earliest stages of the server start-up. It is read by the server much earlier than server.xml so it can affect the start-up and behavior of the Open Liberty kernel right from the start. |
| resources/           |                                                                       Shared resource definitions: adapters, data sources, are injected into `wlp/usr/shared/resources/`. Can be referenced in server.xml using `${shared.resource.dir}` property.                                                                        |

For more info:  
https://openliberty.io/docs/ref/config/serverConfiguration.html
https://www.ibm.com/support/knowledgecenter/en/SSD28V_liberty/com.ibm.websphere.wlp.core.doc/ae/rwlp_dirs.html
