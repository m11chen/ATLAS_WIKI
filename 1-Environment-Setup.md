## Table of Contents

* [System Requirements](#system-requirements)
* [Ubuntu Linux Workstation Setup](#ubuntu-linux-workstation-setup)
  * [Installing Ubuntu WSL on Windows 10](#installing-ubuntu-wsl-on-windows-10)
  * [Installing Java and Python Dependencies](#installing-java-and-python-dependencies)
  * [Additional Ubuntu Dependencies](#additional-ubuntu-dependencies)
* [Ixia Windows Host Setup](#ixia-windows-host-setup)
  * [Installing Ixia Explorer](#installing-ixia-explorer)
  * [Installing Bitvise SSH Server](#installing-bitvise-ssh-server)

---

## System Requirements

The ATLAS slave node contains of a Linux workstation PC and an Ixia host PC for controlling the Traffic Generator (TG). Operation of the Ixia TG depends on Windows Dynamically-Linked Libraries of IxExplorer, so the Ixia TG host requires a Windows environment. Hence, an Ubuntu  operating system running inside the Windows Subsystem for Linux (WSL) is recommended as a dual system integrated slave node PC. It is also possible to use the WSL environment independently or a pure Ubuntu Linux environment as the workstation; a second dedicated Windows PC installed with IxExplorer and SSH server may be used as the TG host.

## Ubuntu Linux Workstation Setup

This section guides you through the process of setting up a Ubuntu environment as the slave node workstation. If you are running a pure Linux environment, skip ahead to the [install Java and Python dependencies](#installing-java-and-python-dependencies) section.

## Installing Ubuntu WSL on Windows 10

The core developers and test engineers for ATLAS use Ubuntu 20.04 running on Windows 10 WSL2 as the workstation. In order to setup this environment, it is necessary to update Windows to the Insider Preview version and upgrade the WSL kernel. Follow these instructions to upgrade Windows and install the latest WSL2 environment.

 1. Download and run Windows 10 Update Assistant
    * (https://www.microsoft.com/en-us/software-download/windows10)
 2. Join the Windows Insider Program
    1. Click on the "Start" button.
    2. Open "Settings".
    3. Go to "Update & Security".
    4. Click on "Windows Insider Program".
    5. Login with your Microsoft Account.
    6. Select "Update frequency": slow.
    7. Restart your computer.
 3. Update Windows to Latest Version
    1. Click on the "Start" button.
    2. Open "Settings".
    3. Go to "Update & Security".
    4. Click on "Windows Update".
    5. Install the latest Windows Insider Preview version; this may take a few minutes.
    6. Restart your computer.
 4. Turn On Virtual Machine and WSL Feature
    1. Search and open "Windows Features" from the start menu.
    2. Enable "Windows Subsystem for Linux" and "Virtual Machine Platform".
    3. Restart your computer.
 5. Install Ubuntu 20.04
    1. Open "Microsoft Store".
    2. Search and install [Ubuntu 20.04](https://www.microsoft.com/en-us/p/ubuntu-2004-lts/9n6svws3rx71?activetab=pivot:overviewtab)
    3. Set Ubuntu username/password; default configuration is "accuser/accuser".
    4. Navigate to Windows Update again and install all newly available updates; make sure to upgrade to a version higher than 2004 which supports WSL2.
 6. Install the WSL2 Linux kernel update package
    * Download and install [WSL update](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
 7. Enable BIOS Virtualization Technology (Intel VT-x, AMD-v)
    1. Restart your computer and enter the BIOS.
    2. Enable virtualization technology.
 8. Configure Ubuntu 20.04 to use WSL 2
    1. Open "Windows Powershell".
    2. Check Ubuntu WSL version:
        ```
        PS  wsl -l -v
        ```
    3. Set Ubuntu 20.04 to use WSL2:
        ```
        PS  wsl --set-version Ubuntu-20.04 2
        ```
    4. Set WSL2 to be the default WSL version:
        ```
        PS  wsl --set-default-version 2
        ```
    5. Launch Ubuntu 20.04:
        ```
        PS  ubuntu2004.exe
        ```
    6. Exit Ubuntu and restart your computer.
    7. Proceed to the next section to setup your Ubuntu environment.

## Installing Java and Python Dependencies
    
The core ATLAS framework is implemented using Python 3, and launching the Jenkins slave node requires the Java Runtime Environment. Follow these steps to install the Java default JDK, Python 3, and other Python dependencies To use the Ubuntu Linux environment as the Jenkins slave node workstation:

1. Update and upgrade your Linux environment.
    * Login as root user and run the following commands:
      ```
      sudo apt-get update
      sudo apt-get upgrade
      ```
2. install Java: the default JDK.
    * Run the following command to install the Java JDK:
      ```
      sudo apt-get install default-jdk
      ```
2. install Python 3.
    ```
    sudo apt-get install python3
    ```
3. Install Python 3 PIP.
    ```
    sudo apt-get install python3-pip
    ```
4. Install pexpect version 4.8.0.
    ```
    sudo pip3 install pexpect==4.8.0
    ```
5. Install pyserial version 3.4.
    ```
    sudo pip3 install pyserial==3.4
    ```
6. Install pexpect_serial version 0.0.4.
    ```
    sudo pip3 install pexpect_serial==0.0.4
    ```
7. Install netaddr version 0.7.19.
    ```
    sudo pip3 install netaddr==0.7.19
    ```
8. Install netifaces version 0.10.9.
    ```
    sudo pip3 install netifaces==0.10.9
    ```
9. Install openpyxl version 3.0.3.
    ```
    sudo pip3 install openpyxl==3.0.3
    ```
10. Install pyshark package.
    ```
    sudo pip3 install pyshark
    ```

## Additional Ubuntu Dependencies

Several additional dependencies are required by running the "apt-get install" command. they are listed below.

1. Install Git:
    ```
    sudo apt-get install git
    ```
2. Install SNMP:
    ```
    sudo apt-get install snmp
    ```
3. Install plink Putty command-line tool:
    ```
    sudo apt-get install putty-tools
    ```
4. Install tshark command-line tool:
    ```
    sudo apt-get install tshark
    ```
5. Install Tcl:
    ```
    sudo apt-get install tcl
    ```

## Ixia Windows Host Setup

As of May 2020, ATLAS support for using a pure Windows slave node is deprecated. To setup the Windows environment for hosting the Ixia traffic generator APIs, only follow the steps in the "Installing Ixia Explorer" and "Installing Bitvise SSH Server" sections.

## Installing Ixia Explorer

If Ixia Explorer is not already installed on your Windows host, Obtain and run the Ixia Explorer installation executable corresponding to the version supported by the traffic generator in your test environment.

## Installing Bitvise SSH Server

The Ixia TG host communicates with the workstation through SSH. If using a WSL environment as the workstation, bothe the workstation and TG host requires installation of the Bitvise SSH server. If using a pure Linux workstation, then only install Bitvise on the TG host. Follow these steps to install Bitvise SSH server and add a Windows virtual user account:

1. Install Bitvise SSH Server.
    * Run the Bitvise SSH server setup file in the ./dependencies/win32/ directory.
2. After installation completes, open the Bitvise SSH Server Control Panel.
3. Click on "Open easy settings".
4. Select the "Virtual accounts" tab.
5. Click the "Add" button.
6. Enter a user name for the "Virtual account. (default username is "accuser")
7. Click "Virtual account password" and Type in a password for the account. (default password is "accuser")
8. Check the box for "Login allowed".
9. Check the box for "Allow file transfer".
10. For the "Shell access type" combo-box, select "Command Prompt"
11. for "Virtual filesystem layout", select "Allow full access".
12. Enter the path for the "Root directory"; the default path is:
    ```
    C:\Users\BvSsh_VirtualUsers
    ```
13. Click "Ok".
14. Click "Save changes" to close the Easy Settings dialog.

---

**ATLAS Project Readme**    
**Copyright(c) Accton Technology Corporation, 2019, 2020.**
