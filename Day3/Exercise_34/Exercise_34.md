<table width=100% border=>
<tr><td colspan=2><img src="images/spacer.png"></td></tr>
<tr><td colspan=2><h1>EXERCISE 3_4 - ML Foundation Re-trainable services</h1></td></tr>
<tr><td><h3>SAP Partner Workshop</h3></td><td><h1><img src="images/clock.png"> &nbsp;60 min</h1></td></tr>
</table>


## Description
In this exercise you will learn how to 'fine-tune' a generic machine learning model to a custom use case. You can use this service without being a Machine Learning Expert. The base model is provided by SAP. The Retrain Service adapts the base model to a specific one using training data provided by the user: thus, the classification results for the user data-set are greatly improved compared to the generic base model.

In this exercise you will:

-	Install SAP CF CLI (for more info on how to install CF CLI go to: <https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/en-US/b8ee7894fe0b4df5b78f61dd1ac178ee.html>) 
-	Login to SAP CF using the SAP CF CLI
-	Upload the training data. (Note: the training data is already uploaded for this exercise)
-	Perform retraining process using the SAPML CLI tool
-	Deploy the retrained model using the SAPML CLI tool
-	Use the deployed retrained model for inference (with a prepared SAPUI5 app)

## Target group

* Developers
* People interested in SAP Leonardo and Machine Learning 


## Goal

The goal of this exercise is to understand how retrain a generic machine learning model to a custom use case.


## Prerequisites
  
Here below are prerequisites for this exercise.

* Completed previous exercises
* A set of test images you will use for testing your retrained service. Please download the zip file from here [RetrainImages.zip](files/RetrainImages.zip?raw=true) and uncompress it in a proper folder on your machine
* The ML Service key you already created in the previous exercise
* The data for retraining (already uploaded to the file system associated with your Cloud foundry ML Foundation service account. This is a step that you need to perform as well in a productive environment or POC. We skipped it during this workshop as it would take some time)
* As the retraining service is running in the productive Cloud Foundry environment, you will need an Access Token to run the service. Please use the Generate Token helper tool in order to get the OAuth2 token
* (OPTIONAL) In case you want to explore how to retrain your service from the CLI you need also to install the SAPML CLI Plugin as explained later in the optional part 


## Steps

1. [Push retraining app to CF](#push-retraining-app)
1. [Manage training data](#manage-training-data)
1. [Retrain the Image Classifier Model using the Retraining App](#retrain-with-app)
1. [Deploy the retrained model](#deploy-retrained-model)
1. [Test the retrained Model](#test-retrained-model)
1. [(Optional) Retrain and Deploy the retrained Image Classifier Model with SAPML CLI plugin](#retrain-and-deploy-with-cli)


### <a name="push-retraining-app"></a> Push retraining app to CF
The first operation is to push the retraining app to CF. This will be used later in the exercise.

1.	First, download the file [retraining-app-master.zip](files/retraining-app-master.zip?raw=true) and extract it on your machine  
	![](images/01.png)

1. Open the file *manifest.yml* with your favourite editor

1. Change the name, the services and the service broker accordingly and save the file  
	![](images/02.png)

1. Open a terminal window, navigate to your project's folder and run the following command to login to your Cloud Foundry environment

	```sh
	cf login -a <YOUR_API_ENDPOINT>  -u <YOUR_CLOUD_FOUNDRY_USER>
	```
where **\<YOUR\_API\_ENDPOINT\>** is the hostname you can read when you go inside your subaccount on your Cloud Platform environment and **\<YOUR\_CLOUD\_FOUNDRY\_USER\>** is the username provided by your instructor. When running this command you will be requested to enter the related password and to choose the right organization. Please choose the organization named **ml\_train\_XX** where **XX** is your workstation ID
	![](images/03.png)

1. Once done, enter the command `cf push` to push the Cloud Foundry application to your Cloud Foundry space
	![](images/04.png)

1. When the process finishes you should get some information about the pushed app. You should see that the application is started and running and you should also get the URL to reach this application. Copy this URL in the clipboard
	![](images/05.png)

1. Add the "https://" protocol string at the beginning and paste the copied URL in your browser
	![](images/06.png)

1. The Retraining application has been successfully deployed to CF.



### <a name="manage-training-data"></a> Manage training data
In this section you will learn how to manage and navigate in your training data. Remeber that the training data have been already uploaded for you in the space under the folder "Brands".

1. Click on the tile named **Manage Training Data**  
	![](images/07.png)

1. Click on the *Brands/* folder  
	![](images/08.png)

1. Here you see the folder structure with *test*, *training* and *validation* sub folders. Click on the first sub folder named *Brands/test/*  
	![](images/09.png)

1. Within each sub folder you find the 5 categories: Adidas, Apple, BMW, Coca-Cola and SAP. In each of these the related images are stored  
	![](images/10.png)

1. Click on the **back** arrow to navigate back to the home page  
	![](images/11.png)

1. You have successfully explored the structure of the training data in the file system.



### <a name="retrain-with-app"></a> Retrain the Image Classifier Model using the Retraining App
In this section you will learn how to retrain the Image Classifier Model using the Retraining App we have just deployed to Cloud Foundry.

1. Navigate to **Manage Retraining Jobs**
	![](images/12.png)

1. Create a new retraining job by clicking the **+** sign.   
	![](images/13.png)

1. Choose the *Brands/* folder, enter the model name as **brands-XXX** where **XXX** is your workstation ID and click on **Create**
	![](images/14.png)

1. The job is started. Click on the back arrow to go back to the main page
	![](images/15.png)

1. On this page you can see that there is a **PENDING** job; looking at the toolbar, you can select to view the jobs in their different states
	![](images/16.png)

1. After a certain time (refresh the page from time to time by clicking on the refresh button located in the top right corner of the list) you can see that the job you submitted is now in the status of **SUCCEEDED**. Click on its row in the list
	![](images/17.png)

1. Here you see some details of your re-training job: start date, end date, number of epochs, name and version of your new model
	![](images/18.png)

1. You have successfully retrained your model.



### <a name="deploy-retrained-model"></a> Deploy the retrained model

1. From the home page, click on the **Manage Retrained Models** tile
	![](images/19.png)

1. A list of models with their versions is shown. Locate the one you have just submitted and click on the **Deploy** button
	![](images/20.png)

1. In case there is another model already deployed, confirm that you want to undeploy it first
	![](images/21.png)

1. A new deployment job will be put in the status of **PENDING**. From time to time, click on the refresh button in the application
	![](images/22.png)

1. After a certain time the job will succeed and marked as **SUCCEEDED**
	![](images/23.png)

1. Click on the deployment row and you will get some further information about its status
	![](images/24.png)

1. You have successfully deployed the retrained model to ML Foundation service.



### <a name="test-retrained-model"></a> Test the retrained model
In order to test the retrained model you need to open the SAPUI5 application you have already used in the previous exercise. This application must be modified a little bit in order to produce the expected results.

1. First of all, be sure you have available the following two things: the service key and the application located here <https://generate_ml_token.cfapps.eu10.hana.ondemand.com/> for generating the authentication token
	![](images/25.png)
	![](images/26.png)

1. Once you have ensured you have this information, go to <https://account.hana.com/cockpit> using the credentials provided by your instructor; then click on the **Trial Home** button   
	![](images/27.png)

1. Click on **Neo Trial** to go to your Neo landscape
	![](images/28.png)

1. Go to the **Destinations** item in the left side menu and click on **New Destination**
	![](images/29.png)

1. Enter the following parameters, where the URL is represented by the **IMAGE\_CLASSIFICATION\_URL** you can findin the service key

	| Parameter      | Value                                                             |
	| -------------- | ----------------------------------------------------------------- |
	| Name           | retraining                                                        |
	| Type           | HTTP                                                              |
	| Description    | Retraining                                                        |
	| URL            | [the IMAGE_CLASSIFICATION URL you retrieved from the service key] |
	| Proxy Type     | Internet                                                          |
	| Authentication | NoAuthentication                                                  |

	![](images/30.png)

1. Open SAP Web IDE with the SAPUI5 application you have already used in the previous exercise, and edit the *settings.json* file by adding the following code

	```json
	, {
			"name": "Retrained Image Classifier",
			"url": "/retraining/models/<your_model_name>/versions/<version>",
			"headers": {
				"Authorization": "<<<< YOUR BEARER TOKEN >>>>"
			}
		}
	```
	![](images/31.png)

1. Go to the page where you have your authentication token and click on the **Copy To Clipboard** button
	![](images/32.png)

1. Replace the "<<<< YOUR BEARER TOKEN >>>>" text with the token you have in the clipboard and then also replace the model name and version placeholders with the ones related to the model you have just deployed; then **save** the file

	![](images/33.png)

1. Run the application by clicking on the "play" button in the toolbar
	![](images/34.png)

1. When the application starts, drang & drop one of the images contained in the [RetrainImages.zip](files/RetrainImages.zip?raw=true) directly on the blue camera picture. For example, take BMW.jpg
	![](images/35.png)

1. You should be able to see a new column containing the results of the classification based on the retrained model: you can appreciate the accuracy of those results, also for the other images in the zip file

	>NOTE: Scores shown here could be different from yours

	![](images/36.png)

1. Congratulations! You have successfully retrained, deployed and tested the Image Classification model.


### <a name="retrain-and-deploy-with-cli"></a> (Optional) Retrain and Deploy the retrained Image Classifier Model with SAPML CLI plugin
In this part, you will learn how to do the same things you have done with the UI, this time with the SAP Machine Learning Foundation Command Line Interface. 


1.	First, let's check if your Cloud Foundry CLI is correctly installed. Open a Terminal window and enter 
	
	```sh
	cf -v
	``` 
	
	You should get a version equal or greater to the one showed in the picture. If not, you need to install the latest CF ClI as explained in the prerequisites to this workshop  
	![](images/40.png)

1. Now, login to SAP Cloud Platform with the command

	```sh
	cf login -a <YOUR_API_ENDPOINT> -u <YOUR_USER>
	```
	where **\<YOUR\_API\_ENDPOINT\>** must be replaced with the endpoint URL you can get when you navigate in the cockpit to your subaccount and **\<YOUR\_USER\>** with the user provided by your instructor  
	![](images/41.png)

1. Enter the password for your user and in case you are requested to choose an organization, please choose the one named **ml-trainXX**, where **XX** is your workstation ID
	>NOTE: The password will not be shown  
	
	![](images/42.png)

1.	Enter the command `cf service-key <INSTANCE_NAME> <SERVICE_KEY_NAME>` to display your service key and make sure that everything is available. If you have followed the naming convention we used in the previous exercise, your command should be 

	```sh
	cf service-key ml_instance_XX ml_servicekey_XX
	```
	
	where **XX** must be replaced by your workstation ID. You can copy this service key somewere for your comfort because it will be needed later in this exercise  
	![](images/43.png)

1. Now we need to install the Cloud Foundry SAPML Plugin. First of all, you need to check if you have already have installed it previously. Run the command

	```sh
	cf plugins
	```
	
	to get the list of all the installed plugins. If you see that SAPML is already installed as in this case do the next step(to uninstall) otherwise skip it  
	![](images/44.png)

1. Run the command 
	
	```sh
	cf uninstall-plugin SAPML
	```
	to **uninstall** the old plugin  
	![](images/45.png)

1.	Download the SAPML CF CLI plugin from the page <https://tools.hana.ondemand.com/#mlfoundation>. This CLI plugin is an extension of standard CF plugin. We have added a subcommand called `sapml` to easily interact with our ML Foundation APIs. Take the one specific to your platform and extract it in a proper location on your machine

	![](images/46.png)
	
1. Then from the Terminal window run the command
	
	```sh
	cf install-plugin -f <path/to/plugin/plugin_file> 
	```
	After the installation, with the command `cf plugins` you can check if the SAPML plugin is correctly installed  
	
	![](images/47.png)

1.	Enter the command: 

	```sh
	cf sapml config get
	```
	You will see that some values are not set, after you installed the SAPML plugin. In case you used it before, make sure that the values point to the right API endpoints, see next steps  
	![](images/48.png)
	
1.	Set the correct values for your SAPML configuration using the following commands (you need to take the missing values in the "<>" brackets from your Service Key) 
	
	```sh
	cf sapml config set job_api <JOB_SUBMISSION_API_URL>
	cf sapml config set retraining_image_api <IMAGE_RETRAIN_API_URL>
	cf sapml config set retraining_text_api <TEXT_LINEAR_RETRAIN_API_URL>
	cf sapml config set auth_server <url>
	```

1. When done, enter
	
	```sh
	cf sapml config get
	```
	
	to check that everything is set correctly  
	![](images/49.png)

1. Initialize the cloud filesystem with the command
	
	```sh
	cf sapml fs init 
	```
	![](images/50.png)

1. List the root directory with the command

	```sh
	cf sapml fs list
	```

	Display the subfolders of the already available "Brands" training data
	
	```sh
	cf sapml fs list Brands/
	```
	
	Display the training categories
	
	```sh
	cf sapml fs list Brands/training/
	```
	![](images/51.png)

1. You need now to prepare a configuration file for retraining. Open your favourite text editor and create a new file named *retrain.json*

1. Copy the text below and paste it in this new file, replacing **XX** with your workstation ID  

	```json
	{
  		"dataset": "Brands",
  		"modelName": "brands-XX",
  		"learningRate": 0.001
	}
	```

	Let me give you a bit of explanation about the content of this file. You need to specify the data set name and a model name. As our folder structure has the root name "Brands", we choose the value "Brands" for our dataset. Give it a new model name of your choice and save it (e.g. brands-xx). You can add optional retrain parameters (e.g. learning rate). Once finished editing, remember to **save** the file  
	![](images/52.png)

1. Start the retrain process using the SAPML CLI command

	```sh
	cf sapml retraining job_submit path/to/retrain.json -m image
	```
	![](images/53.png)
	
1. Check the job status: it could be PENDING  

	```sh
	cf sapml retraining jobs -m image
	```
	![](images/54.png)

1. After a while your job should finish successfully: be patient, it might take some time  
	![](images/55.png)

1. List the directory to get the name of the executed job

	```sh
	cf sapml fs list
	```
	![](images/56.png)

1. Download the retrain log by specifying the job name in its path and the local path where you want to put the downloaded log

	```sh
	cf sapml fs get [REMOTE_PATH] [LOCAL_PATH]
	```
	![](images/57.png)

1. Display the retrain log by opening it with your favourite text editor. Please whait until the job is finished successfully and **take note of the version number at the bottom of the log (in your case it should be 1)**. This version is incremented for every retrain run when using the same model name. The model name is the "brand-XX" string (remember to replace **XX** with your workstation ID) you have previously specified in the *retrain.json* file

	![](images/58.png)

1. Deploy the retrained model by specifying the model name and version

	```sh
	cf sapml retraining model_deploy [MODEL_NAME] [MODEL_VERSION_NUMBER] -m image
	```
	![](images/59.png)

1. Check the deployment status: it takes a couple of minutes until the model container with the retrained model is up and running. At the end of the process you should get a deployment status "SUCCEEDED" message.

	```sh
	cf sapml retraining model_deployments -m image
	```
	![](images/60.png)

1. Once your deployment is in place, in order to test it, you need to perform the same steps listed in the previous chapter.



## Summary
You have completed the exercise!
You are now able to:

* Use CF commands to create ML FOUNDATION SERVICE instance and service key
* Use SAPML CF CLI plugin to interact with ML FOUNDATION SERVICE APIs
* Retrain your Model 
* Deploy your retrained Model 
* Use the retrained Model for inference

Please proceed with next exercise.
