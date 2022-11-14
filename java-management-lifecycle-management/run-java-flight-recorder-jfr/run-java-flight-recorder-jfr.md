# Run Java Flight Recorder (JFR)

## Introduction

This lab walks you through the steps to run Java Flight Recorder (JFR) on any java app on your Fleet.

Estimated Time: 15 mins

### Objectives

In this lab, you will:

* Create a Java Flight Recorder Work Request using the Java Management Service console interface.
* View and monitor the status of Work Requests created using the Java Management Service console interface.
* Download the generated JFR file for further analysis.



### Prerequisites

* You have signed up for an account with Oracle Cloud Infrastructure and have received your sign-in credentials.
* You are using an Oracle Linux image or Windows OS on your Managed Instance for this workshop.
* Access to the cloud environment and resources configured in [Lab 1](?lab=set-up-and-enable-lcm-on-jms).

## Task 1: Submit Java Flight Recorder Work Request

1. First, open the navigation menu, click **Observability & Management**, and then click **Fleets** under **Java Management**. Select the fleet that you are interested in.
  
    ![image of console navigation to java management service](images/console-navigation-jms-fleet.png)

2. Scroll down and under **Resources**, select **Managed instances**. You should see a list of Managed instances that are currently in your Fleet. Select the Managed instance you are interested in.
  
    ![image of fleet details page with crypto event analysis button](images/fleet-managed-instances.png)

3. Scroll down and under **Resources**, select **Applications**. You should see a list of Java applications running in this managed instance. Select the Java application you want to run with JFR and click **Run Java Flight Recorder** button.
  
    ![image of crypto event run settings](images/managed-instance-applications-run-jfr.png)

4. Select **Select from default profiles** for **Recording options** and from the dropdown box, choose **Default.jfc**.
  
    For **Max recording duration** lower it to **5 mins** and keep **Max recording size** as is at **500MB**. Click **Start** to begin the JFR recording.
  
    ![image of jfr configs before starting](images/jfr-config-start.png)

5. Scroll down and under **Resources**, select **Work requests**. You should see a list of the Work Requests that are currently in your Fleet. **Java Flight Recorder** that was started should be at the top of the list.
  
    ![image of work request](images/jfr-work-request-started.png)

    >**Note:** If you have set the **Max recording duration** to 5 mins It will take approximately 10 mins for the request to be completed.

6. Wait for the work request to show as complete.
  
    ![image of work request completed](images/jfr-work-request-completed.png)

7. Once the work request shows as complete, from the **Fleet** details page by clicking the **Object storage bucket** name under **Object storage**

    ![image of crypto event run settings](images/fleet-bucket-link.png)

8. Your **Java Flight Recorder** recording's raw copy is stored in the folder [PENDING CONTENT]

    ![image of crypto event analysis bucket object](images/broken.png)


9. You can open the **Java Flight Recorder** recording in your favorite JFR viewer or you can use the Oracle's **JDK Mission Control** to view th files. 

    JMC download link: https://www.oracle.com/java/technologies/jdk-mission-control.html

 You may now **proceed to the next lab.**




## Acknowledgements

* **Author** - 
* **Last Updated By** - Somik Khan, November 2022
