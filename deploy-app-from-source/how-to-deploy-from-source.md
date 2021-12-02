# Steps to deploy the app.js application from source

1. Make sure you've completed all the [pre-requisite steps](https://github.com/uwefassnacht/code-engine-samples/blob/main/README.md#pre-requisite-steps) that are necessary for any deployment to IBM Cloud Code Engine

2. If you are not already, [log in to the IBM Cloud](https://cloud.ibm.com/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login)

3. Configure the build process. More details and options are [here](https://cloud.ibm.com/docs/codeengine?topic=codeengine-build-image#build-create-cli).

Make sure to replace <mynamespace> in the following command with the name of the namespace that you created in the prerequisit steps.

ibmcloud ce build create --name helloworld-build --image icr.io/<mynamespace>/codeengine-helloworld --registry-secret myregistry --source <https://github.com/uwefassnacht/code-engine-samples> --context-dir deploy-app-from-source/hello-world-app --strategy buildpacks

Now we've got all the pieces in place to submit the actual build run (which will combine the source code with a buildpack, create the image and then store it in our container registry)

ibmcloud ce buildrun submit --build helloworld-build

At this point we have packaged the source code in a container image and stored it in the IBM Cloud Container Registry. We are now ready to deploy the container to Code Engine (make sure to replace <sha> with the has of the container image that was created in the previous step)

ibmcloud ce application create --name hello-world --image --image icr.io/<mynamespace>/codeengine-helloworld --registry-secret myregistry

Add instructions how to test your app via it's URL or CURL.

And you're done! Congratulations, your source-code has been containerized, stored in IBM's container registry and succesfully deployed to IBM Cloud Engine.

## Cleaning Up

Once you're done, you might want to clean up everything you've created. Here are the steps:

1. Remove the Code Engine project you created. This will automatically delete the running application you created

TODO: the project and key creation should probably go into the pre-requisite steps. They are needed for both deployment from source and image.
