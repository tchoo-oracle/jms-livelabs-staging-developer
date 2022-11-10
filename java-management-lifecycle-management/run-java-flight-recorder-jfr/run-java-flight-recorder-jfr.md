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
  <br><br>
  ![image of console navigation to java management service](images/console-navigation-jms-fleet.png)

2. Scroll down and under **Resources**, select **Managed instances**. You should see a list of Managed instances that are currently in your Fleet. Select the Managed instance you are interested in.
  <br><br>
  ![image of fleet details page with crypto event analysis button](images/fleet-managed-instances.png)

3. Scroll down and under **Resources**, select **Applications**. You should see a list of Java applications running in this managed instance. Select the Java application you want to run with JFR and click **Run Java Flight Recorder** button.
  <br><br>
  ![image of crypto event run settings](images/managed-instance-applications-run-jfr.png)

4. Select **Select from default profiles** for **Recording options** and from the dropdown box, choose **Default.jfc**.
  <br><br>
  For **Max recording duration** lower it to **5 mins** and keep **Max recording size** as is at **500MB**. Click **Start** to begin the JFR recording.
  <br><br>
  ![image of work requets](images/jfr-config-start.png)

5. Scroll down and under **Resources**, select **Work requests**. You should see a list of the Work Requests that are currently in your Fleet. **Java Flight Recorder** that was started should be at the top of the list.
  <br><br>
  ![image of work requets](images/jfr-work-request.png)

6. Once the work request shows as complete, open the navigation menu again, click **Storage**, and then click **Buckets** under **Object Storage & Archive Storage**.
  <br><br>
  ![image of console navigation to bucket](images/console-navigation-bucket.png)
  <br><br>
  Select the bucket that starts with **jms_** followed by the **OCID** of your fleet that you are interested in.
  <br><br>
  ![image of fleet bucket](images/fleet-bucket.png)
  <br><br>
  > **Note:** You can also access it from the **Fleet** details page by clicking the **Object storage bucket** name under **Object storage**
  >
  >![image of crypto event run settings](images/fleet-bucket-link.png)

6. Download the **Java Flight Recorder** report by clicking XZXZXZXZXZXZ
  <br><br>
  ![image of crypto event analysis bucket object](images/VAVAVbrokenVAVAV.png)



 You may now **proceed to the next lab.**




## Acknowledgements

* **Author** - 
* **Last Updated By** - Somik Khan, November 2022
