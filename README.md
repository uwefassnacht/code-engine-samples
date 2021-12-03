# Code Engine Samples

This repo contains a few simple examples to deploy to [IBM Cloud Code Engine](https://www.ibm.com/cloud/code-engine)

## Pre-Requisite Steps

Before deploying your code or container to Code Engine, you need to prep by installing a few CLIs (command line interfaces) and creating a container registry (a place to store the containers that Code Engine will be building for you). Once that's done, you create a project in Code Engine and will authorize it to access the container registry.

1. If you don't already have an IBM Cloud account, you need to [register and create one](https://cloud.ibm.com/registration).

2. Next, install the IBM Cloud CLI, which allows you to execute commands against the IBM Cloud from your command line (instead of using the GUI). The instructions are [here](https://cloud.ibm.com/docs/cli?topic=cli-install-ibmcloud-cli).

3. While you're at it, you also need to install two plug-ins for the IBM Cloud CLI. The first one allows you to interact with Code Engine, the second one will give you the ability to work with the IBM Cloud Container Registry

    a) [Installing the IBM Code Engine CLI](https://cloud.ibm.com/docs/codeengine?topic=codeengine-install-cli)

    b) [Installing the IBM Container Registry CLI](https://cloud.ibm.com/docs/Registry?topic=Registry-registry_setup_cli_namespace)

4. You are now ready to log in to the IBM Cloud, using the CLI that we downloaded in step 3.

    ```bash
    ibmcloud login
    ```

5. Once you're logged in, set your registry region to "global" (making your image visible from all regions world-wide).

    ```bash
    ibmcloud cr region-set global
    ```

6. Then create a namespace (a place to hold your container images) in the container registry. Replace `<mynamespace>` with a memorable name. I like to use a concatination of my first and last name.

    ```bash
    ibmcloud cr namespace-add <mynamespace>
    ```

7. Now that you have a place to store your containers, let's turn to Code Engine and create a "place to run" your code. This is called a "project", and here is how you create it (using the Code Engine CLI):

    ```bash
    ibmcloud ce project create --name hello-world-project
    ```

8. Next we need a way for Code Engine to access the containers in your registry namespace. To do thi, you need to create a secret, which you will then use in the next step to acces the IBM Container Registry. See [the docs](https://cloud.ibm.com/docs/codeengine?topic=codeengine-add-registry) for more details. The following command will create a secret called `my-cli-api-key` and save it in a file called `key_file`.

    ```bash
    ibmcloud iam api-key-create cliapikey -d "my-cli-api-key" --file key_file
    ```

9. Now you'll link the registry (named "myregistry") to your Code Engine project. Make sure to replace "API_KEY" with the actual key that you received during the previous step (it's called `apikey`in the`key_file`)
    ```bash
    ibmcloud ce registry create --name myregistry --server icr.io --username iamapikey --password API_KEY
    ```

At this point you are done and have prepared everything you need for your first deployment to Code Engine.

You can now chose whether to deploy your application from source code (meaning you don't have your application containerized) or whether to deploy a container that you have already created outside of Code Engine.

## Deploying an Application from its Source Code

If you don't have your source code containerized yet, Code Engine will do it for you.

[Here](https://github.com/uwefassnacht/code-engine-samples/blob/main/deploy-app-from-source/how-to-deploy-from-source.md) is the sequence of steps to deploy your application from its source code.

## Deploying an Application that is already containerized

If you already have a containerized application, you can deploy the container directly to Code Engine. 

[Here](https://github.com/uwefassnacht/code-engine-samples/blob/main/deploy-app-from-container/how-to-deploy-container.md) is the sequence of steps to do it.

## Cleaning Up the Pre-Requisits

Once you're done, you might want to clean up and remove everything you created. Here are the commands:

```bash
ibmcloud cr region-set global
```

```bash
ibmcloud cr namespace-rm <mynamespace>
```

```bash
ibmcloud ce project delete --name hello-world-project
```
