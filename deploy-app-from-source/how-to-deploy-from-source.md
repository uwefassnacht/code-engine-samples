# Steps to deploy the app.js application from source

1. Make sure you've completed all the [pre-requisite steps](https://github.com/uwefassnacht/code-engine-samples/blob/main/README.md#pre-requisite-steps) that are necessary for any deployment to IBM Cloud Code Engine

2. [Log in to the IBM Cloud](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login)

First we'll create a Code Engine project (and name it "hello-world-project")

ibmcloud ce project create --name hello-world-project

Create a secret, which we will use in the next step to acces the IBM Container Registry. See [the docs](https://cloud.ibm.com/docs/codeengine?topic=codeengine-add-registry) for more details. The actual secret will be saved in a file called "key_file".

ibmcloud iam api-key-create cliapikey -d "My CLI APIkey" --file key_file

Now we'll create the registry (named "myregistry") using the IBM Container Registry service. Make sure to replace "API_KEY" with the actual key that you received during hte previous step.

ibmcloud ce registry create --name myregistry --server icr.io --username iamapikey --password API_KEY

Next we configure the build process. More details and options are [here](https://cloud.ibm.com/docs/codeengine?topic=codeengine-build-image#build-create-cli).

Make sure to replace <mynamespace> in the following command with the name of the namespace that you created in the prerequisit steps.

ibmcloud ce build create --name helloworld-build --image icr.io/<mynamespace>/codeengine-helloworld --registry-secret myregistry --source <https://github.com/uwefassnacht/code-engine-samples> --context-dir deploy-app-from-source/hello-world-app --strategy buildpacks

Now we've got all the pieces in place to submit the actual build run (which will combine the source code with a buildpack, create the image and then store it in our container registry)

ibmcloud ce buildrun submit --build helloworld-build

At this point we have packaged the source code in a container image and stored it in the IBM Cloud Container Registry. We are now ready to deploy the container to Code Engine (make sure to replace <sha> with the has of the container image that was created in the previous step)

ibmcloud ce application create --name hello-world --image --image icr.io/<mynamespace>/codeengine-helloworld --registry-secret myregistry
