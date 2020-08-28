Welcome to the ATLAS Project Wiki. Here you will find the full documentation of the project including instructions for setting up and running your own test environment. General information about the program's implementation and code examples are also provided if you wish to contribute to the project by becoming a developer.

---

## Table of Contents

1. [Environment Setup](1-environment-setup)
    * [System Requirements](1-environment-setup#system--requirements)
    * [Ubuntu Linux Workstation Setup](1-environment-setup#ubuntu-linux-workstation-setup)
      * [Installing Ubuntu WSL on Windows 10](1-environment-setup#installing-ubuntu-wsl-on-windows-10)
      * [Installing Java and Python Dependencies](1-environment-setup#installing-java-and-python-dependencies)
      * [Additional Ubuntu Dependencies](1-environment-setup#additional-ubuntu-dependencies)
    * [Ixia Windows Host Setup](1-environment-setup#ixia-windows-host-setup)
      * [Installing Ixia Explorer](1-environment-setup#installing-ixia-explorer)
      * [Installing Bitvise SSH Server](1-environment-setup#installing-bitvise-ssh-server)
2. [Implementation Guideline](2-implementation-guideline)
    * [Introduction](2-implementation-guideline#introduction)
    * [General Programming Style Guide](2-implementation-guideline#general-programming-style-guide)
      * [Glossary of Project Components](2-implementation-guideline#glossary-of-project-components)
      * [Object Naming Rules](2-implementation-guideline#object-naming-rules)
      * [Docstring and Syntax Rules](2-implementation-guideline#docstring-and-syntax-rules)
    * [Directory Structure and Module Overview](2-implementation-guideline#directory-structure-and-module-overview)
      * [config](2-implementation-guideline#config)
      * [core](2-implementation-guideline#core)
        * [config.py](2-implementation-guideline#config.py)
        * [report.py](2-implementation-guideline#report.py)
        * [topology.py](2-implementation-guideline#topology.py)
        * [tshark.py](2-implementation-guideline#tshark.py)
        * [ui_script.py](2-implementation-guideline#ui_script.py)
          * [User-Interface Class](2-implementation-guideline#user---interface-class)
          * [Script Class](2-implementation-guideline#script-class)
      * [io](2-implementation-guideline#io)
      * [lib](2-implementation-guideline#lib)
        * [Lib Example 1 - CLI_SysMgmt.py](2-implementation-guideline#lib-example-1---cli_sysmgmt.py)
        * [Lib Example 2 - CLI_ACL.py](2-implementation-guideline#lib-example-2---cli_acl.py)
      * [scripts](2-implementation-guideline#scripts)
        * [Script Example 1 - IPv6_ACL_0010.py](2-implementation-guideline#script-example-1---ipv6_acl_0010.py)
        * [Script Example 2 - VLAN_Trunk_0060.py](2-implementation-guideline#script-example-2---vlan_trunk_0060.py)
      * [main.py](2-implementation-guideline#main.py)
3. [User Manual](3-user-manual)
    * [Getting Started](3-user-manual#getting-started)
    * [Creating Jenkins Slave Node](3-user-manual#creating-jenkins-slave-node)
    * [Creating Jenkins Free-Style Job](3-user-manual#creating-jenkins-free---style-job)
    * [Cloning source repository and Creating Config Directory](3-user-manual#cloning-source-repository-and-creating-config-directory)
    * [Execution of Main Module](3-user-manual#execution-of-main-module)
      * [Mandatory Options](3-user-manual#mandatory-options)
      * [Non-Mandatory Options](3-user-manual#non---mandatory-options)
      * [Config Settings Diagnostics](3-user-manual#config-settings-diagnostics)
      * [Executing Tests from Jenkins Job](3-user-manual#executing-tests-from-jenkins-job)
    * [Helpful Hints for Developers](3-user-manual#helpful-hints-for-developers)
4. [Config File](4-config-file)
    * [Config File Structure](5-config-file#config-file-structure)
    * [Setup JSON File Editor](5-config-file#setup-json-file-editor)
    * [Config File](5-config-file#config-file)
      * [PDU](5-config-file#pdu)
      * [TG](5-config-file#tg)
      * [DUT_-model](5-config-file#dut_-model)
      * [DUT1, DUT2, DUT3](5-config-file#dut1-dut2-dut3)
      * [Topology](5-config-file#topology)
      * [Feature](5-config-file#feature)

---

**ATLAS Project Readme**
**Copyright(c) Accton Technology Corporation, 2019, 2020.**
