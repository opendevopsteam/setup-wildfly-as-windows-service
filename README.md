# setup-wildfly-as-windows-service
🛠 Setup WildFly as Windows Service

WildFly application server is configured via DevOps sometimes that means they need to configure as windows service and is another technique for running the server on background. There are multiple steps as follows:

**Step 1:**

*Copy the files to WildFly directory*

1.  Go to *WILDFLY_HOME_DIR*/docs/contrib/scripts
1.  Copy the **service** directory to C:\Servers\wildfly-11.0.0.Final\bin

**Step 2:**

*Update the Settings in the Service Batch File*

The file that is copied to the current WildFly directory is **service.bat** file and is ready to modified.

1.  Go to *WILDFLY_HOME_DIR*\bin
1.  Open **service.bat** file and open it in Edit Mode with Notepad++

**Tip:** The **DISPLAYNAME** will appear in Windows Services list so if administrator is ready to install multiple servers on the same windows machine then wants to update these settings with something more meaningful as described below:

`If developers team have DEV and PROD environment that may want to update the settings to the following:`

```
DISPLAYNAME=WildFly Application Server 11.0.0.Final Development
DISPLAYNAME=WildFly Application Server 11.0.0 Final Production
```


`Note: The SHORTNAME needs to be unique`

**Optional:** To prevent memory loss issues that administrator needs to modify the `JAVA_OPTS` setting to increase the memory.

```
Current Setting: JAVAOPTS=-Xrs
New Setting: JAVAOPTS=-Xmx1024M -Xms512M -XX:MaxPermSize=512M -Xrs
```

**Step 3:**

*Install the Windows Service*

1.  Open and Run Windows CMD as Administrator
1.  Go to WildFly bin directory

`$ cd WILDFLY_HOME_DIR\bin`

**Note:** If the administrator have set **JBOSS_HOME** system variable, it can be replaced with the WildFly directory PATH with **%JBOSS_HOME%**`

Enter the following command to install the service:

`$ service install`

**Step 4:**

*Update the Windows Service Properties*

1.  Go to **Start** and select **Control Panel**
1.  Click on **System and Security** in the Control Panel
1.  Click on **Administrative Tools**
1.  Double-click on **Services** to display a list of local services install on the current server
1.  Scroll to the service called *WildFly Application Server*
1.  Right-click on the name of the service to display the pop-up menu
1.  Select **Properties** to open the *Properties Window*
1.  At **Startup Type**, select Automatic.
1.  Click on **Start** to start WildFly services
1.  The dialog window will open displaying the progress of starting the service and once the startup process is complete, click **OK** to save the changes.

`Check and verify if Windows Service is started properly of WildFly Server.`

**Important:** The actual name that appears in the list of services depends on what you entered in the *service.bat* file for the **DISPLAYNAME** settings

**Step 5:**

Last but not least, the last configuration should be modified to be accessible to all IP addresses.

*  Go to `standalone.xml` configuration file that is saved into `C:\Servers\wildfly-11.0.0.Final\standalone\configuration` PATH and open with Notepad++ or Visual Studio Code

```
<interface name="public">
    <inet-address value="${jboss.bind.address:0.0.0.0}"/>
</interface>
```

> READY
