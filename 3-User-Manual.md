## Table of Contents

* [Getting Started](#getting-started)
* [Creating Jenkins Slave Node](#creating-jenkins-slave-node)
* [Creating Jenkins Free-Style Job](#creating-jenkins-free---style-job)
* [Cloning source repository and Creating Config Directory](#cloning-source-repository-and-creating-config-directory)
* [Execution of Main Module](#execution-of-main-module)
  * [Mandatory Options](#mandatory-options)
  * [Non-Mandatory Options](#non---mandatory-options)
  * [Config Settings Diagnostics](#config-settings-diagnostics)
  * [Executing Tests from Jenkins Job](#executing-tests-from-jenkins-job)
* [Helpful Hints for Developers](#helpful-hints-for-developers)
---

## Getting Started

ATLAS is designed to be deployed from the Jenkins master server onto the local slave node for testing. A slave node consists of DUT, workstation, TG host, traffic generator, power delivery unit, and other environment software and hardware dependencies. Among these, the workstation and TG host only require active internet connection to the management port of the DUT and traffic generator respectively, so they may be physically located in a remote environment away from the other hardware test equipment. See the environment setup section for the system requirements and the configuration instructions for the workstation and TG host. The following sections describe the process of setting up your local workstation as part of the Jenkins slave node testing environment.

## Creating Jenkins Slave Node

Before remote testing is possible, the local workstation needs to be identified by the Jenkins server to be a slave node workstation.  follow these instructions to create a new slave node for your PC on the Jenkins master server:

1. Open the [Jenkins master server dashboard](http://10.100.41.88:8080/). Log in with your username and password.
2. Click on "Manage Jenkins".
3. Click on "Manage Nodes".
4. Click on "New Node".
5. Enter a node name. The default format is Location_Username_Computer-Name; for example: "Taipei_Michael_DemoPC".
6. Select "Permanent Agent", then click on "OK".
7. Enter a description for your node. You may include the location and components of your test environment, such as "Remote workstation for SONiC-Ixia test lab in Hsinchu
8. Enter a "Remote root directory" using the full path; for example: "/mnt/d/Taipei_Michael_DemoPC".
9. For "Launch method", select "Launch agent by connecting it to the master".
10. Check the box for " Tool Locations".
11. Click on the "Add" button.
12. For the tool "Name", select "(Git) Default".
13. In the "Home" text field, enter the path to Git on your workstation. This is usually "/usr/bin/git"; you may verify this by typing "whereis git" in the Bash terminal.
14. Click the "Save" button.

## Creating Jenkins Free-Style Job

The basic method of executing a test cycle on Jenkins is using the free-style job. The following instructions creates a new job and assigns the slave node you created in the previous step for its execution:

1. Open the [Jenkins master server dashboard](http://10.100.41.88:8080/). Log in with your username and password.
  2. Click on "New Item".
3. Enter an item name; for example "ATLAS_Taipei".
4. Select "Freestyle project", then click "OK".
5. Enter a description for the job.
6. Check the box for "GitHub project".
7. For "Project url", enter the following:
    ```
    https://github.com/m11chen/ATLAS_Project.git
    ```
8. Check the box for "Restrict where this project can be run", and in the "Label Expression" text field, enter the node name you created in the previous section; for example "Taipei_Michael_DemoPC".
9. For "Source Code Management", select "Git".
10. In the "Repository URL" text field, enter again the following:
    ```
    https://github.com/m11chen/ATLAS_Project.git
    ```
11. For "Credentials", select "m11chen/****** (github_atas_mike)".
12. Click the "Save" button.

## Cloning source repository and Creating Config Directory

Now you have both the Jenkins slave node and job set up, you can run this job to clone the source repository to your local workstation and create a config directory for your test environment.

 1. Download "agent.jar:".
    1. Open the [Jenkins dashboard](http://10.100.41.88:8080/) again.
    2. In the table of nodes, find your node name and click on its hyperlink to open its "Agent" page.
    3. Under the "Connect agent to Jenkins one of these ways" section, find the "Or if the agent is headless" item. Click on the "agent.jar" link to download the file.
 2. Launch the slave node via Java Network Launching Protocol (JNLP).
    1. Place the "agent.jar" in your "/home/<username>/" root directory.
    2. Copy the entire command from the same item "Or if the agent is headless" from the previous step. It should look something like this:
      ```
      java -jar agent.jar -jnlpUrl \
      http://10.100.41.88:8080/computer/Taipei_Michael_DemoPC/slave-agent.jnlp -workDir \
      "/mnt/d/Taipei_Michael_DemoPC"
      ```
    3. Open a terminal and navigate to your root directory, the same path as where you put the agent.jar file.
    4. Paste and run the command from step 2 above after "sudo ", type in your sudo password, then type enter to establish the JNLP connection.
    5. Wait for the terminal to show the "connected message"; it looks something like this:
      ```
      May 22, 2020 6:07:33 PM hudson.remoting.jnlp.Main$CuiListener status
      INFO: Connected
      ```
    6. Leave this terminal open for the duration of the test cycle.
 3. Creating a config directory for your test environment.
    1. Open the Jenkins job page you created in the previous section.
    2. Click on "Build Now".
    3. After the build shows "success", navigate to the Remote root directory" you created on step 8 in the "Creating Jenkins Slave Node" section.
    4. Find the "workspace/<job_name>/src/config" directory.
    5. Create a copy of the "default" config and rename the directory to your slave node name. The default format is the same as the node name: Location_Username_Computer-Name, such as "Taipei_Michael_DemoPC".
 4. Configuring environment variables.
    1. Open "system.json" and type in the values for each key corresponding to your environment. The workstation IP, username and password should match the scp server you configured during environment setup.
    2. Contact the administrator of the TG host to obtain the IP address, username, and password of the TG host in your test environment.
    3. For each DUT required for testing, edit the DUT1/DUT2/DUT3.json files with corresponding values for your environment.
    4. Edit DUT_model.json with your DUT settings.
    5. Edit TG.json with the chassis, slot, port, and other settings for your traffic generator.
    6. Edit PDU.json with your power delivery unit settings.

## Execution of Main Module

The program entry point of ATLAS is through the main module. The following options are available when executing main:

### Mandatory Options
* '-p' - Platform of the device under test (DUT).
* '-m' - Model name of DUT.
* '-ws' - slave node config name for the workstation
* '-tc' - Relative path to the Test Design (TD) package or the test case script module.

### Non-Mandatory Options

* '-rd' - RD system include json file location.
* '-tg' - traffic generator model
* '-pdu' - power delivery unit model
* '-r' - test retry count when encountering ERROR/FAIL/Check
* '-td' - root directory for the value of -tc option
* '-nv' - do not get DUT version
* '-nt' - do not deploy topology
* '-br' - build report
* '\<verbosity level\>' - only 1 may be specified:
  * '-n' -- No output. If a previous log file exists for the test case, a prompt appears asking whether to delete existing log files.
  * '-v' -- low verbosity; only script name, test result, and test duration is displayed on the screen, and the complete log is written to file.
  * '-vv' -- Medium verbosity; script name, test result, test duration, along with detailed log paths are displayed on the screen. The log is only written to file; a value may be given after the option flag to specify a specific directory to store the log files.
  * '-vvv' -- High verbosity. This is the default option if no verbosity level is provided. Log both appears on the terminal and is written to file; a value may be given after the option flag to specify a specific directory to store the log files.

### Config Settings Diagnostics

If all environment configurations are entered correctly in the config files, you should now be able to run a few basic diagnostics test cases in the common.T1.Diagnostics package:

* One or more DUT user-interface  tests - Diag_Console.py, Diag_ConsoleServer.py, Diag_SSH.py, Diag_Telnet.py, Diag_SNMP.py.
* Workstation command line interface test - Diag_Workstation.py
* Traffic generator test - Diag_TG.py

Select the ones applicable to your test environment and run the following command from the /src source code root directory, replacing the option values as required for your test environment. For example, to run Diag_ConsoleServer.py, Diag_Workstation.py, and Diag_TG.py, you would type:
  ```
  sudo python3 main.py -p <platform> -m <model> -ws Taipei_Michael_DemoPC -tc \
  "common/T1/Diagnostics/Diag_ConsoleServer.py, Diag_Workstation.py, Diag_TG.py"
  ```
After execution completes, the test logs is saved to the /log directory in the Git root and automatically archived with timestamp.

### Executing Tests from Jenkins Job

Now that all environment configurations are verified, you are ready to run tests using the Jenkins free-style job you prepared earlier.

1. Open the [Jenkins master server dashboard](http://10.100.41.88:8080/). Log in with your username and password.
2. In the table of jobs, find and click on the job you created; for example, "ATLAS_Taipei_Demo".
3. On the job project page, click on "Configure".
4. On the configure page, find and click on the "Add build step" button.
5. In the list of actions, click on "Execute shell".
6. In the "Command" text field, type in the command for executing main.py.
    * For example, to run the diagnostics again from the previous section withminimal verbosity while building a report:
      ```
      cd src
      sudo python3 main.py -v -br -p sonic -m <model> -ws Taipei_Michael_DemoPC -tc \
      "common/T1/Diagnostics/Diag_ConsoleServer.py, Diag_Workstation.py, Diag_TG.py"
      ```
    * To run SONiC T1 functions CoS, Incremental_Config, and IPv6_ACL in addition to the diagnostics, you could use this syntax:
      ```
      cd src
      sudo python3 main.py -v -br -p <platform> -m <model> -ws Taipei_Michael_DemoPC -tc \
      "['common/T1/Diagnostics/Diag_ConsoleServer.py, Diag_Workstation.py, Diag_TG.py', \
      'sonic/T1/CoS, Incremental_Config, IPv6_ACL']"
      ```
7. Click on the "Save" button.
8. Verify the JNLP agent you used to clone the Git repository is still online. If not, re-execute the JNLP command before proceeding to the next step.
9. Click on "Build Now" to start the test cycle.

## Helpful Hints for Developers

If you are a developer for ATLAS, it is helpful to use the "alias" feature of the Linux Bash shell along with the '-td' option to specify the function you are working on to save some typing. For example, when working on IPv6_ACL test cases, you can define an alias in the .bashrc file located at your user root directory (cd ~):
  ```
  alias ipv6="./main.py -p sonic -m AS5835 -ws Taipei_Michael_DemoPC -td \
  sonic/T1/IPv6_ACL -tc"
  ```
Using this alias, you only need to type the following to execute IPv6_ACL_0010.py:
  ```
  ipv6 IPv6_ACL_0010.py
  ```
Or, to run IPv6_ACL_0030.py and IPv6_ACL_0040.py:
  ```
  ipv6 "IPv6_ACL_0030.py, IPv6_ACL_0040.py"
  ```
And, to execute all IPv6_ACL cases:
  ```
  ipv6 .
  ```
Similarly, you can create an alias to cold-restart your DUT by sending SNMP command to the PDU:
  ```
  alias restart5835-2="snmpset -v 2c -c private 10.100.41.99 .1.3.6.1.4.1.21317.1.3.2.2.2.2.4.0 i 4"
  ```

---

**ATLAS Project Readme**    
**Copyright(c) Accton Technology Corporation, 2019, 2020.**
