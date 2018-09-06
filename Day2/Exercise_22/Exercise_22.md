<table width=100% border=>
<tr><td colspan=2><h1>EXERCISE 2_2 - DEVELOP A FRONT-END APPLICATION USING APPLICATION PROGRAMMING MODEL</h1></td></tr>
<tr><td><h3>SAP Partner Workshop</h3></td><td><h1><img src="images/clock.png"> &nbsp;60 min</h1></td></tr>
</table>


## Description
In this exercise, you’ll learn how to 

* create a new application using the Application Programming Model template
* create a service and a frontend application with SAP Web IDE Full-Stack.


For further reading on SAP S/4HANA Cloud SDK, click link below.
<https://www.sap.com/germany/developer/topics/s4hana-cloud-sdk.html>


## Target group

* Developers
* People interested in learning about S/4HANA extension and SDK  


## Goal

The goal of this exercise is to create a frontend application for your service using the Application Programming Model template in SAP Web IDE Full-Stack.  

## Prerequisites
  
Here below are prerequisites for this exercise.

* A trial account on the SAP Cloud Platform. You can get one by registering [here](https://account.hanatrial.ondemand.com)
* Previous exercises completed
* A S/4HANA system with a working communication arrangement for the Business Partners collection
* Change the Destination you defined [here](https://github.com/techpmsap/S4HC_Leonardo_PW2018/blob/master/Day1/Exercise_15/Exercise_15.md#-deploy-your-application-to-neo-and-configure-the-destination) by adding 3 further additional properties

	| Property | Value |
	| -------- | ----- |
	| WebIDEEnabled | True |
	| WebIDESystem| ErpQueryEndpoint |
	| WebIDEUsage | odata_gen |
	![](images/pre01.png)

* Write down your Cloud Foundry API Endpoint which you can get by navigating here [here](https://account.hanatrial.ondemand.com), clicking on the **Cloud Foudry Trial** tile and then on the **trial** one. This will be used along with this exercise
	![](images/pre02.png)


## Steps

1. [Create a new application with the Programming Model template](#new-application)
1. [Integrate an external service in the project](#external-service)
1. [Create the custom CRUD classes](#crud-classes)
1. [Build and deploy the project](#build-deploy)
1. [Add some annotations to the service](#add-annotations)
1. [Create the front end application, build and deploy again](#frontend-application)




### <a name="new-application"></a>Create a new application with the Programming Model template

In this chapter you are going to see how to use SAP Web IDE to create a new application with the Application Programming Model. 

1. Login to the Trial Landscape <https://account.hanatrial.ondemand.com/cockpit> with your credentials  
	![](images/01.png)

1. Go to your Cloud Foundry space and delete all the existing applications  
	![](images/02.png)

1. Duplicate the current tab in your browser  
	![](images/03.png)

1. Within the new tab, click on the first breadcrumbs link you see on the top of your screen: it's your home  
	![](images/04.png)

1. Click on the **Neo Trial** tile to access your Neo landscape  
	![](images/05.png)

1. Click on **Services** on the left hand side, search for "web" and click on the **SAP Web IDE Full-Stack** tile  
	![](images/06.png)

1. Click on **Go to Service** to open SAP Web IDE Full-Stack  
	![](images/07.png)

1. You should get this screen when you clikc on the Develpment tab on left hand side  
	![](images/08.png)

1. From the main menu, select **File -> New -> Project from Template**  
	![](images/09.png)

1. Under the **Featured** category select the **SAP Cloud Platform Business Application** template and click **Next**  
	![](images/10.png)

1. Enter a project name (i.e. "bpr_bam") and click **Next**  
	![](images/11.png)

1. Keep other fields as proposed, just change the package to "com.sap.sample.bpr_bam" and click **Finish**  
	![](images/12.png)

1. In the Workspace now you can see your new project  
	![](images/13.png)

1. Your project is ready: we can build it. In order to do this we need to configure a builder. Right click on the name of the project and choose **Project -> Project Settings**  
	![](images/14.png)

1. Go to the **Cloud Foundry** section and select **Use the following Cloud Foundry configuration**. Choose your API Endpoint (it should be the same you can see when you look at your Subaccount details in the cockpit; then click on **Install Builder**  
	![](images/15.png)

1. After a while, the builder will be installed. Click on the **Save** button. You can also click on **Close** to close this page  
	![](images/16.png)

1. If you look in your Cloud Foundry space now, you should see the builder application (it has a strange name, don't worry about it!) up and running  
	![](images/17.png)

1. Go back to SAP Web IDE and enable the Console with **View -> Console** if not yet enabled  
	![](images/18.png)




### <a name="external-service"></a>Integrate an external service in the project

1. We want now to use an external service like the one for retrieving Business Partners from S/4HANA Cloud. This service is the same we used in the previous exercises. In order to include it in our project we need to right click on the *srv* folder and choose **New -> Data Model from External Service**  
	![](images/19.png)

1. Select **Service URL**, locate the destination named **ErpQueryEndpoint** (pay attention that this destination is the same we defined in the exercise "Retrieving data from S/4HANA and deploying to NEO", so it should be already in place), enter the path `/sap/opu/odata/sap/API_BUSINESS_PARTNER`, click on the **Test** button, verify that the service is available and click on **Next**  
	![](images/20.png)

1. Deselect the "Generate Virtual Data Model classes" checkbox and click on **Finish**  
	![](images/21.png)

1. After a while your service model will be created  
	![](images/22.png)

1. You should see something like this in your SAP Web IDE  
	![](images/23.png)

1. For the scope of this project we don't need the *db* module, so we can get rid of it by simply right clicking on it and selecting **Edit -> Delete**. The *db* module will be remove from the project  
	![](images/24.png)

1. This is what you should see after this operation  
	![](images/25.png)

1. Let's now define our Business Partners service. Double click on the *my-service.cds* file to open it in the editor  
	![](images/26.png)

1. Replace its content the following code and save the file. As you can see we are loading the service definitions from the file *./external/csn/API\_BUSINESS\_PARTNER*. Based on this file we are exposing 4 fields in our service and we are also telling to the service provider that we are going to define **custom handlers** for CRUD requests - we will see this in a bit e.g.: @Read(serviceName = "CrudService", entity = "BusinessPartner") - by specifying the statement **@cds.persistence.skip**  

	```
	using API_BUSINESS_PARTNER as bp from './external/csn/API_BUSINESS_PARTNER';
	
	service CrudService {
		@cds.persistence.skip
	     entity BusinessPartner as projection on bp.A_BusinessPartnerType{
	      BusinessPartner,
	      LastName,
	      FirstName,
	      BusinessPartnerCategory
	     };
	}
	```
	![](images/27.png)

1. After a while you should receive the message **Build of "/bpr_bam" completed.**  
	![](images/28.png)




### <a name="crud-classes"></a>Create the custom CRUD classes

1. Locate the *bpr\_bam* folder under the path *bpr_bam/srv/src/main/java/com/sap/sample/* and, right clicking on it, choose **New -> Folder**  
	![](images/29.png)

1. Create a folder named *commands*  
	![](images/30.png)

1. Create another named *crud*  
	![](images/31.png)

1. Right click on the *commands* folder and create a couple of new Java classes by choosing **New -> Java Class**  
	![](images/32.png)

1. Name these two classes as *BusinessPartnerReadByKeyCommand* and *BusinessPartnerReadCommand*  
	![](images/33.png)

1. Create another Java class under the *crud* folder  
	![](images/34.png)

1. Name this class as *BusinessPartnerRead*  
	![](images/35.png)

1. This is what you should get at the end of this process  
	![](images/36.png)

1. Now, double click on the *BusinessPartnerReadByKeyCommand*, enter the following content and save the file

	```java
	import com.netflix.hystrix.exception.HystrixBadRequestException;
	import com.sap.cloud.sdk.odatav2.connectivity.ODataException;
	import com.sap.cloud.sdk.s4hana.connectivity.ErpCommand;
	import com.sap.cloud.sdk.s4hana.connectivity.ErpConfigContext;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartner;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartnerByKeyFluentHelper;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.services.DefaultBusinessPartnerService;
	
	public class BusinessPartnerReadByKeyCommand extends ErpCommand<BusinessPartner> {
	
	    private final ErpConfigContext erpConfigContext;
	    private String businessPartner;
	
	    public BusinessPartnerReadByKeyCommand(ErpConfigContext erpConfigContext, String businessPartner) {
	        super(BusinessPartnerReadByKeyCommand.class, erpConfigContext);
	        this.businessPartner = businessPartner;
	        this.erpConfigContext = erpConfigContext;
	    }
	
	    @Override
	    protected BusinessPartner run() {
	
	        BusinessPartnerByKeyFluentHelper service = new DefaultBusinessPartnerService()
	                .getBusinessPartnerByKey(businessPartner);
	
	        try {
	            return service.execute(erpConfigContext);
	        } catch (final ODataException e) {
	            throw new HystrixBadRequestException(e.getMessage(), e);
	        }
	    }
	}
	```

	![](images/37.png)

1. Double click on the *BusinessPartnerReadCommand*, enter the following content and save the file

	```java
	import com.netflix.hystrix.exception.HystrixBadRequestException;
	import com.sap.cloud.sdk.odatav2.connectivity.ODataException;
	import com.sap.cloud.sdk.s4hana.connectivity.ErpCommand;
	import com.sap.cloud.sdk.s4hana.connectivity.ErpConfigContext;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.helper.Order;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartner;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartnerField;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartnerFluentHelper;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.services.DefaultBusinessPartnerService;
	import com.sap.cloud.sdk.service.prov.api.request.OrderByExpression;
	
	import java.util.*;
	
	public class BusinessPartnerReadCommand extends ErpCommand<List<BusinessPartner>> {
	
	   
	    private final int top;
	    private final int skip;
	    private final BusinessPartnerField[] selectedProperties;
	    private final List<OrderByExpression> orderByProperties;
	    private final ErpConfigContext erpConfigContext;
	
	
	    public BusinessPartnerReadCommand(ErpConfigContext erpConfigContext, int top, int skip, List<String> properties, List<OrderByExpression> orderByProperties) {
	        super(BusinessPartnerReadCommand.class, erpConfigContext);
	        this.erpConfigContext = erpConfigContext;
	        this.top = top;
	        this.skip = skip;
	        selectedProperties = properties.stream().
	                map(property -> new BusinessPartnerField(property))
	                .toArray(BusinessPartnerField[]::new);
	
	        this.orderByProperties = orderByProperties;
	    }
	
	    @Override
	    protected List<BusinessPartner> run() {
	
	        BusinessPartnerFluentHelper service = new DefaultBusinessPartnerService()
	                .getAllBusinessPartner();
	
	        orderByProperties.stream().forEach(expression -> service.orderBy(new BusinessPartnerField<>(expression.getOrderByProperty()), expression.isDescending() ? Order.DESC : Order.ASC));
	
	        service.select(selectedProperties);
	
	        if (skip > 0)
	            service.skip(skip);
	
	        if (top > 0)
	            service.top(top);
	
	        try {
	            return service.execute(erpConfigContext);
	
	        } catch (final ODataException e) {
	            throw new HystrixBadRequestException(e.getMessage(), e);
	        }
	    }
	}
	```
	![](images/38.png)


1. Finally, double click on the *BusinessPartnerRead*, enter the following content and save the file

	```java
	package com.sap.sample.bpr_bam.crud;
	
	import com.sap.cloud.sdk.service.prov.api.operations.Read;
	import com.sap.cloud.sdk.service.prov.api.response.ReadResponse;
	
	import org.slf4j.Logger;
	import org.slf4j.LoggerFactory;
	
	import java.util.List;
	import java.util.stream.Collectors;
	
	import com.sap.sample.bpr_bam.commands.BusinessPartnerReadByKeyCommand;
	import com.sap.sample.bpr_bam.commands.BusinessPartnerReadCommand;
	import com.sap.cloud.sdk.s4hana.connectivity.ErpConfigContext;
	import com.sap.cloud.sdk.s4hana.datamodel.odata.namespaces.businesspartner.BusinessPartner;
	import com.sap.cloud.sdk.service.prov.api.operations.Query;
	import com.sap.cloud.sdk.service.prov.api.request.QueryRequest;
	import com.sap.cloud.sdk.service.prov.api.request.ReadRequest;
	import com.sap.cloud.sdk.service.prov.api.response.QueryResponse;
	
	
	public class BusinessPartnerRead {
	    private final Logger logger = LoggerFactory.getLogger(this.getClass());
	
	    @Read(serviceName = "CrudService", entity = "BusinessPartner")
	    public ReadResponse readSingleCustomerByKey(ReadRequest readRequest) {
	        logger.debug("Received the following keys: {} ", readRequest.getKeys().entrySet().stream().map(x -> x.getKey() + ":" + x.getValue())
	                .collect(Collectors.joining(" | ")));
	
	        String id = String.valueOf(readRequest.getKeys().get("BusinessPartner"));
	
	        BusinessPartner partner = new BusinessPartnerReadByKeyCommand(new ErpConfigContext("ErpQueryEndpoint"), id).execute();
	
	        ReadResponse readResponse = ReadResponse.setSuccess().setData(partner).response();
	
	        return readResponse;
	    }
	
	    @Query(serviceName = "CrudService", entity = "BusinessPartner")
	    public QueryResponse queryCustomers(QueryRequest qryRequest) {
	
	        List<BusinessPartner> businessPartners = new BusinessPartnerReadCommand(new ErpConfigContext("ErpQueryEndpoint"),
	                qryRequest.getTopOptionValue(),
	                qryRequest.getSkipOptionValue(),
	                qryRequest.getSelectProperties(),
	                qryRequest.getOrderByProperties())
	                .execute();
	
	        QueryResponse queryResponse = QueryResponse.setSuccess().setData(businessPartners).response();
	        return queryResponse;
	    }
	}
	```

	![](images/39.png)

1. As a final step, before we build the project, we need to adjust the deployment settings. So double click on the *mta.yaml* file to open it in the editor  
	![](images/40.png)

1. Adapt the code in this file as the following (you could also copy & paste this in there) and save the file

	```yml
	ID: bpr_bam
	_schema-version: '3.1'
	version: 0.0.1
	
	modules:
	  - name: bpcrud
	    type: java
	    path: srv
	    parameters:
	      disk-quota: 512M
	      memory: 768M
	    provides:
	      - name: srv_api
	        properties:
	          url: '${default-url}'
	    requires:
	      - name: bpr_destination
	      - name: bpr_xsuaa
	
	resources:
	  - name: bpr_destination
	    type: destination
	    description: Destination Service
	  - name: bpr_xsuaa
	    type: com.sap.xs.uaa
	```
	![](images/41.png)




### <a name="build-deploy"></a>Build and deploy the project
1. We can build the project. Right click on the project's name and choose **Build -> Build**  
	![](images/42.png)

1. After a while, you should receive a "BUILD SUCCESS" message  
	![](images/43.png)

1. A new folder named *mta_archives* should be available in the project exporer of your SAP Web IDE  
	![](images/44.png)

1. Right click on the *bpr\_bam\_0.0.1.mtar* package located under this folder and choose **Deploy -> Deploy to SAP Cloud Platform**  
	![](images/45.png)

1. Enter your **Cloud Foundry API Endpoint** with your credentials and click **Deploy**  
	![](images/46.png)

1. After a few minutes the deployment ends and you should receive a message with a button to go straight to the service. Click on the **Open** button  
	![](images/47.png)

1. A new browser page opens up, showing the content of this service. It reports all the available OData Endpoints. For this service we have just one: click on its link  
	![](images/48.png)

1. The endpoint is reached and its definition is shown  
	![](images/49.png)

1. If you append the "BusinessPartner" entity name to this OData Endpoint you will get the list of all available business partners coming from the service  
	![](images/50.png)




### <a name="add-annotations"></a>Add some annotations to the service
1. Before we create our frontend application with the List Report template, we want to create some OData annotations directly on the main service. To do this, right click on the *srv* folder and choose **New -> File**  
	![](images/51.png)

1. Enter *main-annotations-cds* as the name for this new file and click **OK**  
	![](images/52.png)

1. Enter the following content for this file and save it. We are simply annotating two data fields under the UI.LineItem annotation

	```
	using CrudService as cs from './my-service.cds';
	
	annotate cs.BusinessPartner with @(
		UI: {
			LineItem: [ 
				{$Type: 'UI.DataField', Value: LastName, "@UI.Importance": #High},
				{$Type: 'UI.DataField', Value: FirstName, "@UI.Importance": #High}
			]
		}
	);
	```
	![](images/53.png)




### <a name="frontend-application"></a>Create the front end application, build and deploy again

1. Now let's create our frontend application. Right click on the *bpr\_bam* project and choose **New -> HTML5 Module**  
	![](images/54.png)

1. Under the **Featured** category, choose the **List Report Application** template and click **Next**  
	![](images/55.png)

1. Enter "frontend" for both **Module Name** and **Title** fields and click **Next**  
	![](images/56.png)

1. Select **Current Project** as the data source and check that you are able to see the **CrudService** you defined earlier. Then click **Next**  
	![](images/57.png)

1. Click **Next**  
	![](images/58.png)

1. Choose **BusinessPartner** as OData Collection and click **Finish**  
	![](images/59.png)

1. This is what you should get at the end  
	![](images/60.png)

1. Open the *mta.yaml* file in the editor and adjust the *frontend* module as the following. Pay attention to modify the URL in the **TENANT\_HOST\_PATTERN** according to your Cloud Foundry API Endpoint. Save the file

	```yml
	  - name: frontend
	    type: html5
	    path: frontend
	    parameters:
	       disk-quota: 1024M
	       memory: 1024M
	    build-parameters:
	       builder: grunt
	    requires:
	     - name: srv_api
	       group: destinations
	       properties:
	          forwardAuthToken: true
	          strictSSL: false
	          name: srv_api
	          url: ~{url}
	     - name: bpr_xsuaa
	    properties:
	      TENANT_HOST_PATTERN: '^(.*)-trial-dev-frontend.cfapps.eu10.hana.ondemand.com'
	```
	![](images/61.png)

1. Let's build the project again. Right click on the project's name and choose **Build -> Build**  
	![](images/62.png)

1. Your build process should finish successfully  
	![](images/63.png)

1. Since we have only 2GB of space in our Cloud Foundry Trial Landscape, we need to go on the cockpit and temporarily stop the builder application. Stop it and go back to SAP Web IDE  
	![](images/64.png)

1. Right click on the *bpm\_bam\_0.0.1.mtar* file and choose **Deploy -> Deploy to SAP Cloud Platform**  
	![](images/65.png)

1. Enter again your **Cloud Foundry API Endpoint** and the related credentials and click **Deploy**  
	![](images/66.png)

1. After some minutes the deployment ends and you get the chance to open the just deployed frontend application directly from SAP Web IDE. Click on the **Open** button for the frontend application  
	![](images/67.png)

1. The frontend application is opened. Enter your SAP Cloud Platform credentials and click **Log On**  
	![](images/68.png)

1. The launch pad is shown. Click on the **frontend** tile  
	![](images/69.png)

1. The List Report application is shown. Notice that you can already see in the list header the two fields we  defined in the annotation file earlier. Click on **Go**  
	![](images/70.png)

1. The list of business partners is shown (many are empty because they simply don't have any LastName and FirstName)  
	![](images/71.png)

1. Congratulations! You have successfully completed the exercise.



## Summary
This concludes the exercise. You should have learned how to create a new application with the Application Programming Model template in SAP Web IDE Full-Stack and how to create in the same project a frontend application to consume your business partners service. Please proceed with the next exercise.
