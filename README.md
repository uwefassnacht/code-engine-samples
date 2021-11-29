# Code Engine Samples
This repo contains a few simple examples to deploy to [IBM Cloud Code Engine](https://www.ibm.com/cloud/code-engine)

## Pre-requisite steps:
- [Registering for IBM Cloud](https://cloud.ibm.com/registration)
- [Installing the IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cli-install-ibmcloud-cli)
- [Logging in to the IBM Cloud](https://cloud.ibm.com/)
- [Installing the IBM Container Registry CLI](https://cloud.ibm.com/docs/Registry?topic=Registry-registry_setup_cli_namespace)
- [Installing the IBM Code Engine CLI](https://cloud.ibm.com/docs/codeengine?topic=codeengine-install-cli)
- [Registering for the IBM Cloud Container registry](https://www.ibm.com/cloud/container-registry)
- Creating a container registry namespace (make sure to replace <mynamespace> with something else, you might want to use your IBM Cloud account user name)
    ibmcloud cr region-set global
    ibmcloud cr namespace-add <mynamespace>

