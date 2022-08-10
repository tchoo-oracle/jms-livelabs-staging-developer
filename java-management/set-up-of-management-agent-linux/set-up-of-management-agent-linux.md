# Install Management Agent on Linux hosts

## Introduction

This lab walks you through the steps to install a management agent on your host and set up tags for the agent and compute instance to allow Java usage tracking by the Java Management Service (JMS).

Estimated Time: 15 minutes

### Objectives

In this lab, you will:

- Install a management agent on a Linux host
- Verify management agent
- Tag Management Agent and Compute Instance
- Monitor the Java runtime(s) and Java application(s) in JMS

### Prerequisites

- You have signed up for an account with Oracle Cloud Infrastructure and have received your sign-in credentials.
- You are using an Oracle Linux image on your host machine or compute instance for this workshop.
- Access to the cloud environment and resources configured in [Lab 2](?lab=setup-a-fleet).

## Task 1: Install Management Agent

1. Download the installation script on your host, or enter the following command to transfer the installation script for Linux downloaded in [Lab 2](?lab=setup-a-fleet) to the remote host compute instance.

    ```
    <copy>
    scp -i <your-private-key-file> <path-to-installation-script> <username>@<x.x.x.x>:<copy-to-path> 
    </copy>
    ```

2. Connect to your instance using SSH.

3. Enter the following command to change file permissions.

     ```
     <copy>
     chmod +x <copy-to-path>/<installation-script-name>.sh
     </copy>
     ```

4. Enter the following command to run the installation script. The installation may take some time to complete.

     ```
     <copy>
     sudo <copy-to-path>/<installation-script-name>.sh
     </copy>
     ```

## Task 2: Verify Management Agent Installation

1. In the Oracle Cloud Console, open the navigation menu, click **Observability & Management**, and then click **Agents** under **Management Agent**.

  ![image of console navigation to access management agent overview](images/management-agent-overview.png)

2. From the Agents list, look for the agent that was recently installed. This agent should be in the compartment created in [Lab 1](?lab=set-up-oci-for-jms).

   ![image of agents main page](images/agents-main-page-new.png)

## Task 4: Configure Java Usage Tracker

1. Execute the following commands:

     ```
     <copy>
     VERSION=$(sudo ls /opt/oracle/mgmt_agent/agent_inst/config/destinations/OCI/services/jms/)
     </copy>
     ```

     ```
     <copy>
     sudo bash /opt/oracle/mgmt_agent/agent_inst/config/destinations/OCI/services/jms/"${VERSION}"/scripts/setup.sh --force
     </copy>
     ```

2. This script creates the file `/etc/oracle/java/usagetracker.properties` with appropriate permissions. By default, the file contains the following lines:
     ```
     com.oracle.usagetracker.logToFile = /var/log/java/usagetracker.log
     com.oracle.usagetracker.additionalProperties = java.runtime.name
     ```

## Task 5: Check that management agent is tagged with the Fleet OCID

1. In the Oracle Cloud Console, open the navigation menu, click **Observability & Management**, and then click **Fleets** under **Java Management**.

  ![image of console navigation to java management service](images/console-navigation-jms.png)

2. Select the Fleet created in [Lab 2](?lab=setup-a-fleet).


3. Take note of the fleet ocid.

  ![image of fleet ocid](images/check-fleet-ocid.png)

4. In the Oracle Cloud Console, open the navigation menu and click **Observability & Management**, and then click **Agents**.
   ![image of console navigation to management agents](images/console-navigation-agents.png)

5. Select the compartment that the management agent is contained in.

  ![image of agents main page](images/agents-main-page-new.png)

6. Select the management agent to view more details

7. Under **Tags**, the `jms` tag will be indicated to show that the management agent is linked to that fleet. The fleet ocid under the jms tag should be the same fleet ocid noted in Step 3.

  ![image of agents details page](images/tagged-mgmt-agent.png)

8. The management agent has been associated to your fleet in JMS. It will now collect information on your Java runtimes and Java Usage based on the scanning frequency defined in [Lab 2: Set Up a Fleet](?lab=setup-a-fleet).

## Task 6: Verify detection of Java applications and runtimes

For the logging of applications to be visible, Java applications must be run again after the installation of the Management Agent. Now that the Management Agent has been set up in your compute instance, it will be able to detect new Java applications that have been executed. This can be observed in the Oracle Cloud Console.

We shall demonstrate the detection of the Java compiler and HelloWorld application created in [Lab 3](?lab=deploy-a-java-application).

1. First, compile the HelloWorld.java file:

     ```
     <copy>
     javac HelloWorld.java
     </copy>
     ```

   Then execute the HelloWorld application:

     ```
     <copy>
     java HelloWorld
     </copy>
     ```

2. In the Oracle Cloud Console, open the navigation menu, click **Observability & Management**, and then click **Fleets** under **Java Management**.

  ![image of console navigation to java management](images/console-navigation-jms.png)

3. Select the compartment that the fleet is in and click the fleet.

4. Click **Java Runtimes** under **Resources**. If tagging and installation of management agents is successful, Java Runtimes will be indicated on the Fleet Main Page after 5 minutes.

  You should see only one Java Runtime. This corresponds to the Java 8 installation from [Lab 3](?lab=deploy-a-java-application).

  ![image of runtimes after successful installation](images/successful-installation.png)

5. Click **Applications** under **Resources**. You should now see two applications. The first is from the javac compiler command and the second is from the HelloWorld application.

  ![image of applications after successful installation](images/successful-installation-applications.png)

You may now **proceed to the next lab.**

## Troubleshoot Management Agent installation issues

**For Task 1**

- If you are unable to download the management agent software using `wget`, you may download the software from the Oracle Cloud Console to your local machine and transfer it over to your compute using Secure Copy Protocol (SCP).

- First, open the navigation menu, click **Observability & Management**, and then click **Fleets** under **Java Management**. Select the fleet that you have created.
  ![image of console navigation to java management service](images/console-navigation-jms.png)

- Click **Set Up Management Agent**.
  ![image of fleet details page](images/fleet-details-page.png)
- Click **Download management agent software**.
  ![image of set up management agent page](images/fleet-set-up-management-agent.png)
- Open up a **Terminal** window in the local machine where the management agent software file is saved.
- Enter the following command to transfer the management agent software file via scp into the remote host compute instance.

    ```
    <copy>
    scp <full_path_of_file_to_be_transferred_on_local_host> opc@<public_IP_Address>:<full_path_of_remote_directory_transferred_to>
    </copy>
    ```

  - In your compute instance, verify that the file transfer is successful by entering the following. You should see your management agent software file.
    ```
    <copy>
    ls
    </copy>
    ```

**For Task 2**

- Ensure that /usr/share folder has write permissions.
- Uninstall and reinstall the management agent after permissions for the /usr/share folder have been updated.

**For Task 3**

- Transfer the response file to /tmp folder where read permissions are allowed.

## Learn More

- Refer to the [Management Agent Concepts](https://docs.oracle.com/en-us/iaas/management-agents/doc/you-begin.html), [Installation of Management Agents](https://docs.oracle.com/en-us/iaas/management-agents/doc/install-management-agent-chapter.html) and
  [JMS Plugin for Management Agents](https://docs.oracle.com/en-us/iaas/jms/doc/installing-management-agent-java-management-service.html) sections of the JMS documentation for more details.

- Use the [Troubleshooting](https://docs.oracle.com/en-us/iaas/jms/doc/troubleshooting.html#GUID-2D613C72-10F3-4905-A306-4F2673FB1CD3) chapter for explanations on how to diagnose and resolve common problems encountered when installing or using Java Management Service.

- If the problem still persists or if the problem you are facing is not listed, please refer to the [Getting Help and Contacting Support](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/contactingsupport.htm) section or you may open a support service request using the **Help** menu in the Oracle Cloud Console.

## Acknowledgements

- **Author** - Esther Neoh, Java Management Service
- **Last Updated By** - Yixin Wei, June 2022
