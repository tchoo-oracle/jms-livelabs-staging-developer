# Detect Java Libraries

## Introduction

Advanced usage tracking allows you view the list of **Java libraries** associated with the deployed Java Applications in the selected fleet.
This lab walks you through the steps to detect **Java libraries** in your fleet.

Estimated Time: 30 minutes

### Objectives

In this lab, you will:

* Detect Java libraries for deployed Java Web Application on a running Java Server and Java SE application on a managed instance.
* Verify Java libraries detection result.


### Prerequisites

* You have signed up for an account with Oracle Cloud Infrastructure and have received your sign-in credentials.
* You are using an Oracle Linux image on your Managed Instance for this workshop.
* Access to the cloud environment and resources configured in [Lab 4](?lab=track-java-servers).

## Task 1: Detect Java libraries for deployed Java Web Application.

1. In **lab 4**, we have set up the running WebLogic server with sample Java Web Application. Please ensure that WebLogic server is still running.
* You may refer to **Lab 4, Task 3** to restart the WebLogic server if necessary.

2. Open the navigation menu, click **Observability & Management**. Under **Java Management**, select **Fleets**.
 ![image of scan java servers](images/console-navigation-fleet.png)
3. On the fleet details page, click **Scan for Java libraries**.
 ![image of fleet details page](images/scan-java-libraries.png)
If your request is submitted successfully, you should receive a notification in green as seen below: 
![image of scan java servers](images/work-request-of-libraries-scan-created.png)

4. Scroll down the fleet details page, under **Resource** menu, select **Work Request**. You should see the Scan for Java libraries Work Request you submitted in step 2. Wait for the work request to complete.
![image of install java runtime](images/work-request-of-libraries-scan-in-progress.png)

5. If your request is successful, you should see that the Status of the request is marked as **Completed without errors**. It will take approximately 10 minutes for the request to be completed.
![image of install java runtime](images/work-request-of-libraries-scan-completed.png)

6. In the same fleet details page, under **Resource** menu, select **Java libraries** you should see Java libraries that we included in the configuration file for the deployed sample Java Web Application in lab 4.
![image of install java runtime](images/java-libraries-web.png)
* The [CVSS](https://www.oracle.com/security-alerts/cvssscoringsystem.html) score is the indication of the security vulnerability associated with the Java library. The score **varies** over time and there might be new vulnerabilities affecting your application since JMS refreshes data from the National Vulnerability Database(NVD) on a weekly basis.
* There will have **3** categories of CVSS score for Java libraries in the scan result based on availability in NVD as following:
  1. Both Java library is found in NVD and CVSS score is obtained.
  The CVSS score will show normally with score value and severity status of Low(green color), Medium(yellow color), High(red color) as per the range of CVSS score.
  2. Java library is found in NVD but CVSS score is not computed.  
  The **Unknown** will be shown under **CVSS score**.  
  3. Java libraries is not found in NVD.  
  The **N/A** will be shown under **Version** and **No matches found** will be shown under **CVSS score**. 

  ![image of install java runtime](images/java-libraries-categories.png)

7. You can stop the WebLogic server now by pressing **CTRL + c**.
![image of install java runtime](images/stop-weblogic-server.png)

## Task 2: Create Sample Java SE Application.

1. Create a sample Java SE Application.
* If you are not already in the home directory of your compute instance, navigate there with command as following:
```
    <copy>
    cd ~
    </copy>
```
* Create the sample Java SE Application named **GreetingApp** with commands as following:
```
    <copy>
    mvn archetype:generate -DgroupId=com.sample -DartifactId=GreetingApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    </copy>
```
* Navigate to the directory of the created application with following command:
```
    <copy>
    cd ./GreetingApp
    </copy>
```

2. Edit main Java class.
* Open the App.java file with following command:
```
    <copy>
    nano src/main/java/com/sample/App.java
    </copy>
```
* Replace the content as following. Save the file.
```java
    <copy>
package com.sample;
import com.google.gson.Gson;
import java.text.SimpleDateFormat;
import java.util.*;
public class App
{
    public static void main(String[] args)
    {
        System.out.println("Greeting message in format of Json will be printed every 20 secs");
        final Map<String, String> map = new HashMap<String, String>();
        map.put("Message", "Hello, World!");
        Timer t = new Timer();
        t.schedule(new TimerTask() {
            @Override
            public void run() {
                map.put("Time", new SimpleDateFormat("yyyy-MMM-dd HH:mm:ss").format(new java.util.Date()));
                System.out.println(new Gson().toJson(map));
            }
        }, 0, 20000);
    }
}
    </copy>
```
3. Edit pom.xml configuration file.
* Open the pom.xml file with the following command:
```
    <copy>
    nano pom.xml
    </copy>
```
* Replace the content as following. Save the file.
```xml
    
<copy>
	<project
		xmlns="http://maven.apache.org/POM/4.0.0"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
		<modelVersion>4.0.0</modelVersion>
		<groupId>com.sample</groupId>
		<artifactId>GreetingApp </artifactId>
		<packaging>jar</packaging>
		<version>1.0-SNAPSHOT</version>
		<name>AppTest</name>
		<url>http://maven.apache.org</url>
		<dependencies>
			<dependency>
				<groupId>junit</groupId>
				<artifactId>junit</artifactId>
				<version>3.8.1</version>
				<scope>test</scope>
			</dependency>
			<dependency>
				<groupId>com.google.code.gson</groupId>
				<artifactId>gson</artifactId>
				<version>2.9.0</version>
			</dependency>
		</dependencies>
		<build>
			<plugins>
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-assembly-plugin</artifactId>
					<executions>
						<execution>
							<phase>package</phase>
							<goals>
								<goal>single</goal>
							</goals>
							<configuration>
								<archive>
									<manifest>
										<mainClass>com.sample.App</mainClass>
									</manifest>
								</archive>
								<descriptorRefs>
									<descriptorRef>jar-with-dependencies</descriptorRef>
								</descriptorRefs>
							</configuration>
						</execution>
					</executions>
				</plugin>
			</plugins>
		</build>
	</project>
</copy>
```
3. Build Java SE Application as executable jar file using Maven.
```
    <copy>
        mvn clean package
    </copy>
```
* You should be able to see the output similar as following after successful build.
![image of install java runtime](images/build-java-se-app.png)
4. Run Java SE application with following command.
```
    <copy>
        sudo java -jar ./target/GreetingApp-1.0-SNAPSHOT-jar-with-dependencies.jar
    </copy>
```
* You should be able to see the output similar as following after Java SE application started successfully.
![image of install java runtime](images/run-java-se-app.png)

## Task 3: Detect Java libraries for Java SE Application.

1. Follow **Task 1, Step 2-5** to initialize Java libraries scan and it will take approximately 10 mins for the created work request to be completed.

2. Scroll down the fleet details page, under **Resource** menu, select **Java libraries**,  you should be able to see that extra Java library named **gson** that we included in sample Java SE application are added to result now.
![image of install java runtime](images/java-library-gson.png)

3. In the same fleet details page, Click the **gson** library, you should see details of selected library and list of applications that are using the selected library.
![image of install java runtime](images/java-se-app-info.png)
 
4. In the same page, click **GreetingApp-1.0-SNAPSHOT-jar-with-dependencies.jar**, You should see the details of Java SE Application that we run in the previous steps.
![image of install java runtime](images/java-se-app-detail.png)
> **Note:** Tracking of Java Application that is running with **Non-Oracle JDKs** in the fleet is also supported.

5. You can stop the Java SE application by pressing **CTRL + c**.  

You may now **proceed to the next lab.**


## Learn More
* Refer to the [Java Runtime Lifecycle Management](https://docs.oracle.com/en-us/iaas/jms/doc/advanced-features.html#GUID-08673CB1-D87D-4BC5-A61D-E59DCC879ABB), [Work Request](https://docs.oracle.com/en-us/iaas/jms/doc/getting-started-java-management-service.html#GUID-47C63464-BC0C-4059-B552-ED9F33E77ED3) and [Viewing a Work Request](https://docs.oracle.com/en-us/iaas/jms/doc/fleet-views.html#GUID-F649F0E5-DD54-4DEC-A0F1-942FE3552C93) sections of the JMS documentation for more details.

* Use the [Troubleshooting](https://docs.oracle.com/en-us/iaas/jms/doc/troubleshooting.html#GUID-2D613C72-10F3-4905-A306-4F2673FB1CD3) chapter for explanations on how to diagnose and resolve common problems encountered when installing or using Java Management Service.

* If the problem still persists or it is not listed, then refer to the [Getting Help and Contacting Support](https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/contactingsupport.htm) section. You can also open a support service request using the **Help** menu in the OCI console.

## Acknowledgements

* **Author** - Youcheng Li, Java Management Service
* **Last Updated By** - Youcheng Li, November 2022
