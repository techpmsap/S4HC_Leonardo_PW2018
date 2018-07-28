<table width=100% border=>
<tr><td colspan=2><h1>EXERCISE 2_3 - CREATING YOUR OWN COMMUNICATION ARRANGEMENT</h1></td></tr>
<tr><td><h3>SAP Partner Workshop</h3></td><td><h1><img src="images/clock.png"> &nbsp;20 min</h1></td></tr>
</table>


## Description
In this exercise, you’ll learn how

* to create a Communication System along with a Communication User
* to setup a Communication Arrangement
* to test your new Communication Arrangement


## Target group

* Developers
* People interested in learning about S/4HANA extension and SDK  


## Goal

The goal of this exercise is to create a Communication System along with a Communication User and to setup a Communication Arrangement for the Business Partners collection.


## Prerequisites
  
Here below are prerequisites for this exercise.

* A S/4HANA Cloud system with related credentials
* Postman for testing the service. You can download it from [here](https://www.getpostman.com)


## Steps

1. [Create a Communication User and a Communication System](#communication-system)
1. [Create a Communication Arrangement ](#communication-arragnement)
1. [Test the service](#test-the-service)



### <a name="communication-system"></a> Create a Communication User and Communication System
In the upcoming exercises you will learn how to retrieve and work with data using the S/4HANA Cloud SDK. For this reason we want now to create our own data source so that we can improve our application built in the previous exercise making it able to connect with our S/4HANA Cloud back-end. We start creating our own Communication System. A Communication User will be created as well along with the Communication System.

1. Open the S/4HANA Cloud back-end portal by using the link provided by your instructor  
	![](images/01.png)

1. Click on the down arrow on the top right corner of the page and choose **Communication Management**  
	![](images/02.png)

1. Select **Communication Systems**  
	![](images/03.png)

1. Click on **New** to create a new communication system  
	![](images/04.png)

1. Enter **BP\_EXT\_SYSTEM\_\<LOC\>\_XX** for both the System ID and the System Name, replacing **\<LOC\>** with the location's acronym where you are attending the workshop (i.e. WDF for Walldorf) and **XX** with your workstation ID. Both information will be provided you by the instructor. When done click **Create**  
	![](images/05.png)

1. Enter

	| Parameter | Value |
	| --------- | ----- |
	| Host Name | localhost |
	| Logical System | LOCALHOST |
	| UI Host Name | localhost |
	| Business System | BP\_EXT\_SYSTEM\_\<LOC\>\_XX |
	
	(remember to replace **\<LOC\>** with the location's acronym where you are attending the workshop (i.e. WDF for Walldorf) and **XX** with your workstation ID)
	![](images/06.png)

1. Scroll down the page to the **User for Inbound Connection** section and click on the **+** sign to add a new user  
	![](images/07.png)

1. Since your user is not yet present, click on **New User**  
	![](images/08.png)

1. You are now in the page where you can create a new communication user. Enter

	| Parameter | Value |
	| --------- | ----- |
	| User Name | BP\_EXT\_CU\_\<LOC\>\_XX |
	| Description | BP External Communication User \<LOC\> XX |
	| Password | A password with at least 20 chars with a upper case char,numbers and a special character (i.e. "**Welcome2thisworkshop!**") |

(where **\<LOC\>** with the location's acronym where you are attending the workshop and **XX** is your workstation ID)

Then click on the **Create** button  
	![](images/09.png)

1. Your user has been created. Click on the **OK* button to use it as Inbound Communication User  
	![](images/10.png)

1. Your Inbound Communication User is in place. Now let's add the Outbound Communication User. Press the **+** button in the **User for Outbound Communication** section  
	![](images/11.png)

1. Enter the same user and password you created before for the Inbound Communication and keep **User Name and Password** as **Authentication Method**; then click **Create**  
	![](images/12.png)

1. Both Inbound and Outbound Communication Users are in place. Click on **Save** to confirm the Communication System creation 
	![](images/13.png)

1. Click on the **Home** button to go back to the Home Page  
	![](images/14.png)

1. You have successfully created your Communication System.


### <a name="communication-arragnement"></a> Create a Communication Arragnement
The next step is to create a new Communication Arrangement.

1. Click on the **Communication Arrangements** tile  
	![](images/15.png)

1. Click on **New**  
	![](images/16.png)

1. Browse for the existing scenarios  
	![](images/17.png)

1. Select the **SAP\_COM\_0008** scenario  
	![](images/18.png)

1. Enter **BP\_COMM\_ARR\_\<LOC\>\_XX** as Arrangement Name replacing **\<LOC\>** with the location's acronym where you are attending the workshop and **XX** with your workstation ID; then click **Create**  
	![](images/19.png)

1. Browse for a Communication System    
	![](images/20.png)

1. Select the Communication System **BP\_EXT\_SYSTEM\_\<LOC\>\_XX** you created earlier where **\<LOC\>** with the location's acronym where you are attending the workshop and **XX** is your workstation ID
	![](images/21.png)

1. The correct information is loaded automatically. 
	![](images/22.png)

1. Check that also the user name for the Outbound Communication is the correct one  
	![](images/23.png)

1. Expand the first outbound service **Replicate Customers from S/4 System to Client** and **deactivate** it
	![](images/24.png)

1. Do the same for all the other services  
	![](images/25.png)

1. Once you have deactivated all the outbound services click on **Save** to confirm the creation of your communication arrangement  
	![](images/26.png)

1. If you scroll a little up this page, you can find a link for the **Business Partner Integration**: this will be the link which you need to use in the next exercises for retrieving Business Partners data. The credentials are the Communication User you have defined here. Write down this link with its credentials because you will need them in the upcoming exercises
	![](images/27.png)

1. Click on the **Home** button to go back to the Home Page  
	![](images/28.png)

1. You have successfully created your Communication Arrangement.

### <a name="test-the-service"></a> Test the service
In order to test the service provided by your new Communication Arrangement you can put its URL in a REST client like Postman.

1. Download Postman from from [here](https://www.getpostman.com) and install it  
	![](images/29.png)

1. Open Postman and perform the following steps:
	- set **GET** as request method
	- paste the **URL** you got at the end of the previous chapter
	- click on **the Authorization tab**
	- select **Basic Auth**
	- enter your **Communication User** with its password
	- click the **Send** button
	
	![](images/30.png)

1. You should get a screen like this where the the Response Status is **200** and a the list of all the available collections is returned. You should be also able to read collection titles like A_AddressEmailAddress and A_AddressFaxNumber. This means that your service is working fine  
	![](images/31.png)

1. Congratulations! You have successfully created and tested your Communication User, System and Arrangement.

## Summary
This concludes the exercise. You should have learned how to create a Communication System along with a Communication User and how to setup your Communication Arrangement. Please proceed with the next exercise.
