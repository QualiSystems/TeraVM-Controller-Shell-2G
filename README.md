![](https://github.com/QualiSystems/cloudshell-shells-documentaion-templates/blob/master/cloudshell_logo.png)

# **TeraVM Controller 2G Shell**  

Release date: January 2019

Shell version: 1.0.0

Document version: 1.0

# In This Guide

* [Overview](#overview)
* [Downloading the Shell](#downloading-the-shell)
* [Importing and Configuring the Shell](#importing-and-configuring-the-shell)
* [Updating Python Dependencies for Shells](#updating-python-dependencies-for-shells)
* [Associating a CloudShell Service to a Non-Global Domain](#associating-a-cloudshell-service-to-a-non-global-domain)
* [Typical Workflow](#typical-workflow)
* [References](#references)
* [Release Notes](#release-notes)


# Overview
A shell integrates a device model, application or other technology with CloudShell. A shell consists of a data model that defines how the device and its properties are modeled in CloudShell, along with automation that enables interaction with the device via CloudShell.

### Traffic Generator Shells
CloudShell's traffic generator shells enable you to conduct traffic test activities on Devices Under Test (DUT) or Systems Under Test (SUT) from a sandbox. In CloudShell, a traffic generator is typically modeled using a chassis resource, which represents the traffic generator device and ports, and a controller service that runs the chassis commands, such as Load Configuration File, Start Traffic and Get Statistics. Chassis and controllers are modeled by different shells, allowing you to accurately model your real-life architecture. For example, scenarios where the chassis and controller are located on different machines.

For additional information on traffic generator shell architecture, and setting up and using a traffic generator in CloudShell, see the [Traffic Generators Overview](http://help.quali.com/Online%20Help/9.0/Portal/Content/CSP/LAB-MNG/Trffc-Gens.htm?Highlight=traffic%20generator%20overview) online help topic.

### **TeraVM Controller 2G Shell**
The **TeraVM Controller 2G Shell** shell provides you with connectivity and management capabilities such as device structure discovery and power management for the **TeraVM Controller**. 

For more information on the **TeraVM Controller**, see the official **TeraVM** product documentation.

The **TeraVM Controller** provides automation commands to run on the TeraVM Chassis, such as Load Configuration, Start/Stop Traffic, Get Statistics. For more information on the TeraVM Chassis shell, see the following:

* [TeraVM Chassis 2G Shell](https://community.quali.com/repos/3286/teravm-chassis-2g-shell)


### Standard version
The **TeraVM Controller 2G Shell** shell is based on the Traffic Generator Controller Standard version 2.0.0.

For detailed information about the shell’s structure and attributes, see the [Traffic Shell standard](https://github.com/QualiSystems/cloudshell-standards/blob/master/Documentation/Traffic%20Generator%20Controller%20Shell%20Standard.md) in GitHub.

### Requirements

Release: **TeraVM Controller 2G**

▪ TeraVM version: 13.4

▪ CloudShell version: 8.3 Patch 3, 9.0 Patch 2, 9.1 and above

### Data Model

The shell's data model includes all shell metadata, families, and attributes.

### Automation
This section describes the automation (driver) associated with the data model. The shell’s driver is provided as part of the shell package. There are two types of automation processes, Autoload and Resource.  Autoload is executed when creating the resource in the **Inventory** dashboard, while resource commands are run in the sandbox.

For Traffic Generator shells, commands are configured and executed from the controller service in the sandbox, with the exception of the Autoload command, which is executed when creating the resource.

|Command|Description|
|:-----|:-----|
|Autoload|Discovers the chassis, its hierarchy and attributes when creating the resource. The command can be rerun in the **Inventory** dashboard and not in the sandbox, as for other commands.|
|Load Configuration|Loads configuration and reserves ports.<br>Set the command input as follows:<br>* **TeraVM config file** (config_file_name (String)): The configuration file name. Path should include the protocol type, for  example *tftp://10.10.10.10/asdf*.<br>* **Use ports from reservation** (Enum): **True** or **False**. Updates the configuration file with ports from the current reservation based on their **Logical Name** attributes.
|Start Test|Starts traffic test.|
|Stop Test|Stops traffic test.|
|Get Statistics|Gets the test result file and attaches it to the reservation.|

# Downloading the Shell
The **TeraVM Controller 2G** shell is available from the [Quali Community Integrations](https://community.quali.com/integrations) page. 

Download the files into a temporary location on your local machine. 

The shell comprises:

|File name|Description|
|:---|:---|
|TeravmControllerShell2G.zip|TeraVM Controller 2G shell package|
|teravm-offline-dependencies-1.0.0.zip	|Shell Python dependencies (for offline deployments only)|
|Cleanup Reservation.zip|Teardown script for cleaning a reservation.|

## Importing and Configuring the Shell
This section describes how to import the TeraVM Controller 2G shell and configure and modify the shell’s devices. 

### Importing the shell into CloudShell

**To import the shell into CloudShell:**
  1. Make sure you have the shell’s zip package. If not, download the shell from the [Quali Community's Integrations](https://community.quali.com/integrations) page.
  2. In CloudShell Portal, as Global administrator, open the **Manage – Shells** page.
  3. Click **Import**.
  4. In the dialog box, navigate to the shell's zip package, select it and click **Open**.

The shell is displayed in the **Shells** page and can be used by domain administrators in all CloudShell domains to create new inventory resources, as explained in [Adding Inventory Resources](http://help.quali.com/Online%20Help/9.0/Portal/Content/CSP/INVN/Add-Rsrc-Tmplt.htm?Highlight=adding%20inventory%20resources). 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The service can now be added to a blueprint from the **Apps/Service** catalog's **Networking** category.  

### Offline installation of a shell

**Note:** Offline installation instructions are relevant only if CloudShell Execution Server has no access to PyPi. You can skip this section if your execution server has access to PyPi. For additional information, see the online help topic on offline dependencies.

In offline mode, import the shell into CloudShell and place any dependencies in the appropriate dependencies folder. The dependencies folder may differ, depending on the CloudShell version you are using:

* For CloudShell version 8.3 and above, see [Adding Shell and script packages to the local PyPi Server repository](#adding-shell-and-script-packages-to-the-local-pypi-server-repository).

* For CloudShell version 8.2, perform the appropriate procedure: [Adding Shell and script packages to the local PyPi Server repository](#adding-shell-and-script-packages-to-the-local-pypi-server-repository) or [Setting the python pythonOfflineRepositoryPath configuration key](#setting-the-python-pythonofflinerepositorypath-configuration-key).

* For CloudShell versions prior to 8.2, see [Setting the python pythonOfflineRepositoryPath configuration key](#setting-the-python-pythonofflinerepositorypath-configuration-key).

### Adding shell and script packages to the local PyPi Server repository
If your Quali Server and/or execution servers work offline, you will need to copy all required Python packages, including the out-of-the-box ones, to the PyPi Server's repository on the Quali Server computer (by default *C:\Program Files (x86)\QualiSystems\CloudShell\Server\Config\Pypi Server Repository*).

For more information, see [Configuring CloudShell to Execute Python Commands in Offline Mode](http://help.quali.com/Online%20Help/9.0/Portal/Content/Admn/Cnfgr-Pyth-Env-Wrk-Offln.htm?Highlight=Configuring%20CloudShell%20to%20Execute%20Python%20Commands%20in%20Offline%20Mode).

**To add Python packages to the local PyPi Server repository:**
  1. If you haven't created and configured the local PyPi Server repository to work with the execution server, perform the steps in [Add Python packages to the local PyPi Server repository (offlinemode)](http://help.quali.com/Online%20Help/9.0/Portal/Content/Admn/Cnfgr-Pyth-Env-Wrk-Offln.htm?Highlight=offline%20dependencies#Add). 
  
  2. For each shell or script you add into CloudShell, do one of the following (from an online computer):
      * Connect to the Internet and download each dependency specified in the *requirements.txt* file with the following command: 
`pip download -r requirements.txt`. 
     The shell or script's requirements are downloaded as zip files.

      * In the [Quali Community's Integrations](https://community.quali.com/integrations) page, locate the shell and click the shell's **Download** link. In the page that is displayed, from the Downloads area, extract the dependencies package zip file.

3. Place these zip files in the local PyPi Server repository.
 
### Setting the python PythonOfflineRepositoryPath configuration key
Before PyPi Server was introduced as CloudShell’s python package management mechanism, the `PythonOfflineRepositoryPath` key was used to set the default offline package repository on the Quali Server machine, and could be used on specific Execution Server machines to set a different folder. 

**To set the offline python repository:**
1. Download the *teravm-offline-dependencies-1.0.0* file, see [Downloading the Shell](#downloading-the-shell).

2. Unzip it to a local repository. Make sure the execution server has access to this folder. 

3.  On the Quali Server machine, in the *~\CloudShell\Server\customer.config* file, add the following key to specify the path to the default python package folder (for all Execution Servers):  
	`<add key="PythonOfflineRepositoryPath" value="repository 
full path"/>`

4. If you want to override the default folder for a specific Execution Server, on the Execution Server machine, in the *~TestShell\Execution Server\customer.config* file, add the following key:  
	`<add key="PythonOfflineRepositoryPath" value="repository 
full path"/>`

5. Restart the Execution Server.
 
# Updating Python Dependencies for Shells
This section explains how to update your Python dependencies folder. This is required when you upgrade a shell that uses new/updated dependencies. It applies to both online and offline dependencies.

### Updating offline Python dependencies
**To update offline Python dependencies:**
1. Download the latest Python dependencies package zip file locally.

2. Extract the zip file to the suitable offline package folder(s). 

3. Terminate the shell’s instance, as explained [here](http://help.quali.com/Online%20Help/9.0/Portal/Content/CSP/MNG/Mng-Exctn-Srv-Exct.htm#Terminat).

### Updating online Python dependencies
In online mode, the execution server automatically downloads and extracts the appropriate dependencies file to the online Python dependencies repository every time a new instance of the driver or script is created.

**To update online Python dependencies:**
* If there is a live instance of the shell's driver or script, terminate the shell’s instance, as explained [here](http://help.quali.com/Online%20Help/9.0/Portal/Content/CSP/MNG/Mng-Exctn-Srv-Exct.htm#Terminat). If an instance does not exist, the execution server will download the Python dependencies the next time a command of the driver or script runs.

## Associating a CloudShell Service to a Non-Global Domain

In order to expose a service to users of a domain that is not the Global domain, you must associate the service to the domain. To do this, you need to associate the service to a category that is assigned to the domain.

When you import a service shell, most shells are automatically assigned a default service category which is associated with the Global domain. For custom shells, this may not be true.

**To associate the TeraVM Controller 2G service to a non-global domain:**

**Note:** The association process differs depending on the type of shell - second generation (2G) shell or first generation (1G) shell. The instructions below detail the steps for a 2G service shell.

1. (Optional) To associate the service to a new service category(s): 

	**Note:** If you do not want to add a new category(s) to this shell, you can use the default category that comes out-of-the-box (if it exists).
	
	• Modify the *shelldefinition.yaml* file to add a service category(s) to the shell. See the CloudShell Developer Guide’s [Associating categories to a service shell](https://devguide.quali.com/shells/9.0.0/customizing-shells.html#associating-categories-to-a-service-shell) article. Note that when importing the shell into CloudShell, the new categories will be linked automatically with the Global domain.
	
2. Associate the shell’s service category (either the out-of-the-box category or the new category you created in step 1) to a non-Global domain.
	1. In the **Manage** dashboard, click **Categories** from the left sidebar, or **Domains** if you are a domain admin.
	
	2. Select **Services Categories**.
	
	3. Click the service category that is associated with your service shell.
	
	4. In the **Edit Category** dialog box, from the **Domains** drop-down list, select the desired domain(s).
	
	5. Click **Save**.

# Typical Workflow 

**Workflow 1 - Creating a new blueprint**
1. Create a new blueprint.
   1. Log in to CloudShell Portal and create a new blueprint (**Blueprint Catalog>Create Blueprint**).
   2. Give the blueprint a name.
   
2. Add resources and services to the blueprint. 
   1. Click the **Resource** button and add the TeraVM Chassis resource and all needed ports into the diagram. 
   2. Associate the port sub resources with the TeraVM interfaces, by specifying the port attribute **Logical Name**.
   3. Click the **App/Services** tab and add the **TeraVMController** service.
   
3. Add the teardown script *Cleanup Reservation.zip*, provided with the shell, see [Downloading the Shell](#downloading-the-shell). This script runs the **cleanup_reservation** driver command when the reservation ends and releases ports which were used by the reservation. 
   1. Go to the **Scripts** management page **Manage>Scripts>Blueprint**, click **Add New Script** and choose the **Cleanup Reservation.zip** file. 
   2. Click **Edit** for the newly added script and change **Script Type** to **Teardown**.
   3. Return to the blueprint and open properties, **Blueprint>Properties**. 
   4. In the **Driver** section, select **Python Setup & Teardown**, add **Estimated teardown duration** 1 min.
   5. Click **Add Script** and choose **Cleanup Reservation** from the list. 
   6. Click **Update** to save changes.

**Workflow 2 - Running a test**
1. Reserve a blueprint that is configured to run traffic tests, like the one configured in Workflow 1.

2. From the TeraVMController service, run the **Load Configuration** command.
   1. Hover over the TeraVMController service and click **Commands**.
   2. In the **Resource Commands** pane, click the **Load Configuration** command.
   3. In the **TeraVM config file** input, specify the path to your test configuration file. <br>It can be a full path, or relative path under the location specified in the attribute **Test Files Location**, such as *<reservation_id>/test_group.xml* or only *test_group.xml*. Make sure the path is accessible to the Execution Server running the command.
   4. Click **Run**, to load the test configuration to the TeraVM Controller.
   
3. Run the **Start Test** command.

4. Run the **Stop Test** command.

5. Get the test's result file.
   * Run the **Get Result** command.
   * The test's result file is attached to the sandbox.


# References
To download and share integrations, see [Quali Community's Integrations](https://community.quali.com/integrations). 

For instructional training and documentation, see [Quali University](https://www.quali.com/university/).

To suggest an idea for the product, see [Quali's Idea box](https://community.quali.com/ideabox). 

To connect with Quali users and experts from around the world, ask questions and discuss issues, see [Quali's Community forums](https://community.quali.com/forums). 

# Release Notes 

For release updates, see the shell's [GitHub releases page](https://github.com/QualiSystems/TeraVM-Controller-Shell-2G/releases).
