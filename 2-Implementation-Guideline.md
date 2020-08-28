## Table of Contents

* [Introduction](#introduction)
* [General Programming Style Guide](#general-programming-style-guide)
  * [Glossary of Project Components](#glossary-of-project-components)
  * [Object Naming Rules](#object-naming-rules)
  * [Docstring and Syntax Rules](#docstring-and-syntax-rules)
* [Directory Structure and Module Overview](#directory-structure-and-module-overview)
  * [config](#config)
  * [core](#core)
    * [config.py](#config.py)
    * [report.py](#report.py)
    * [topology.py](#topology.py)
    * [tshark.py](#tshark.py)
    * [ui_script.py](#ui_script.py)
      * [User-Interface Class](#user---interface-class)
      * [Script Class](#script-class)
  * [io](#io)
  * [lib](#lib)
    * [Lib Example 1 - CLI_SysMgmt.py](#lib-example-1---cli_sysmgmt.py)
    * [Lib Example 2 - CLI_ACL.py](#lib-example-2---cli_acl.py)
  * [scripts](#scripts)
    * [Script Example 1 - IPv6_ACL_0010.py](#script-example-1---ipv6_acl_0010.py)
    * [Script Example 2 - VLAN_Trunk_0060.py](#script-example-2---vlan_trunk_0060.py)
  * [main.py](#main.py)

---

## Introduction

Welcome to the ATLAS project implementation guideline. This document is meant for software developers and test engineers wishing to extend the features and test coverage of ATLAS. The core framework, libraries, and scripts of ATLAS is implemented using Python version 3.8, and environment configuration files are stored in JavaScript Object Notation (JSON) format. If you are familiar and comfortable with both Python and JSON, you may want to reference the configuration files, example API libraries, and existing scripts and dive right into project implementation while reading this document. If you are new to Python, it is recommended that you first review some basic Python programming. A good place to start is [the Python Tutorial - Python 3.8.1 documentation](https://docs.python.org/3/tutorial/)

## General Programming Style Guide

ATLAS is a collaborative project contributed by remote developers world-wide. In order to encourage effective and efficient code review by your peers and minimize future maintenance effort for the project, some general programming style should be followed:

### Glossary of Project Components

To facilitate precise communication between developers, the following terms assume the listed definitions within the scope of this project.

* module - A Python source code file with .py extension.
* package - A directory containing multiple Python modules.
* core module - A module inside the src/core package.
* API library (or simply, library) - A module inside a sub-directory in the src/lib package.
* script - A module inside a sub-directory in the src/script package.
* config File - A file with .json extension inside  the src/config directory.
* function - A "def" definition at the root level of a module.
* method - A "def" definition inside a class.

### Object Naming Rules

When creating new types of objects for the project, refer to this section for the general naming rule of the newly created object:

1. For API libraries and scripts, use CapitalizedCamelCase and _ as separators for the module file names.
    * For API libraries, prepend the user-interface type [CLI (console, Telnet, SSH, bash), SNMP, TCL, etc.] before the module name. For scripts, append the test case number after the Test Design. For example:
      ```
      CLI_ACL.py
      SNMP_StandardMIB.py
      CoS_0010.py
      ```
    * Define only a single top-level class inside each library and script module. The name of the class should match the module name exactly without the .py extension. For example, in the above modules, you would define the following classes:
      ```
      class CLI_ACL():
      class SNMP_StandardMIB()):
      class CoS_0010():
      ```
2. Use lowerCaseCamelCase for function and method names plus a lower-case word and _ as the separator for an identifier if necessary. For example:
    ```
    def configACLTable():
    def sudo_setHostName(self, host_name, oid='1.3.6.1.2.1.1.5.0'):
    def run_old(self, ):
    ```
3. Use ALL_CAPITAL_WORDS and _ as separators for constants and static dictionaries. For example:
    ```
    UI.DUT1 = UI.CONF[UI.CMD['-m']]['1']
    TRMC_100K = 100000
    ```
4. Use all lower case individual words and _ as separators for variable names. For example:
    ```
    seven = 7
    Var_1 = 'hello world'
    ```
5. Inside the core package, use your own discretion when naming the module. In general, try to use lower case words and _ as separators, similar to variable names.

### Docstring and Syntax Rules

Keep the following rules in mind when writing the Python source code and Docstring comments:

1. Source code maximum document width: 100 characters; insert line breaks when exceeding this width for easier readability, especially for headlines, purposes, procedures, criteria, and comments.
2. Use 2 spaces "  " for each indent level to reduce frequency of line breaks.
3. Use Docstring format with 3 double quotes """ for multiline comments:
    * Declare the object type (module, class, or function) one line below the opening """ on a single line.
    * For each section, append the colon (:) after the title, then followed by a line break, an indent, and then the content.
    * Insert a blank line before the copyright section.
    * For example:
      ```
      """
      Module Name: ATLAS_Programming_Style_Guide.py
      Headline:
        ATLAS Documentation Example Comment Block
      Purpose:
        Coding style guide for the Accton Test Automation System.
      History:
        2020/03/23 - Michael Chen, last updated.
      
      Copyright(c) Accton Technology Corporation, 2019, 2020.
      """
      ```
      * For sub-sections and sub-sub-sections and so on, indent each level and use some type of symbol/numbering to indicate each level. Be consistent with the formatting within each comment block.
      * For example:
      ```
      """
      Function Name: myExampleFunction
      Purpose:
        Example function comment block with sub-sections in the description.
      Input:
        var_1 - variable 1.
          * example_value_1 = 'value 1'
          * example_value_2 = 'value 2'
        var_2 - variable 2.
          * example_value_3 = 'value 3'
      """
      ```
4. When inserting procedure and criteria strings in scripts, use two sequential "UI.log" methods, using the keywords "step" and "Criteria" in the first arguments; for example:
    ```
    UI.log('Step 1',
        'Create one ingress IPv6 ACL table 1 that has 2 rules to bind DUT1PA.',
        'r1. Priority is 10, src-ipv6 address is %s, action is drop.' %ipv6_1,
        'r2. Priority is 20, src-ipv6 address is %s, action is drop.' %ipv6_2)
    UI.log('Criteria 1',
        'ACL table and rule shall be created and show by show acl tbl and show acl rule command.')
    ```
5. Enclose strings with single quotes '...', and surround assignment operators =, +=, -=, *=, /=, **=, and %= with spaces. For example:
    ```
    s = 'hello world'
    j += 3
    ```
6. Surround logical operators ==, != <= >= <, and >  with spaces. For example:
    ```
    if a == 'hello':
      print('world')
    ```
7. Do not surround assignment operators with spaces inside parenthesized expressions. For example:
    ```
    my_obj = MyClass(a='hello', b='world')
    ```
8. Insert single space after commas (,). For example:
    ```
    def myFunction(param1, param2, param3)
    some_str_list = ['egg', 'ham', 'spam']
    ```

## Directory Structure and Module Overview

The root directory of ATLAS source code is the "src" directory inside the Git repository. It contains four sub-directories and one Python module. Below is a brief description of each:

### config

This is the directory that stores all configuration-related information of the DUT and workstation required for testing. The first level directory organizes configuration files by the Jenkins slave node location, username, and computer name. Platform-specific configurations are organized by sub-directories. Other environment device configurations such as Traffic Generators (TG) and Power Delivery Unit (PDU) configurations are also stored in the config root. All configuration files use the JSON format. Refer to the user manual for more detailed explanation on how to setup your DUT and environment configuration files for executing test scripts.

### core

The core package contains modules that constitute the foundation of the test automation framework. Modules inside this package are platform-independent of the device under test and dictate the general format and structure of the API libraries and test case script modules. They also provide function and features required for the initialization of libraries and execution of test case scripts. The core package contains 1 sub-directory and 5 Python modules:

#### config.py

This module contains functions for reading JSON device configuration files and loading them into Python dictionaries. It contains logic for parsing and identifying file data that  merges all config files into a single Python dictionary object, allowing the program to have access to any device and environment variables anywhere in the program. For the SONiC platform, this module also provides functions for initializing, importing, configuring, and exporting the config_db.json configuration database file which can be uploaded to the DUT via a workstation interface.

#### report.py

This module contains the Report class which consolidates and summarizes the test results from execution of main.py. It generates a test report in Excel format with each sheet representing a Test Design (TD) containing hyperlinks to individual log files. When encountering failures, the failed steps during which the failure occured are printed along with the test case information.

#### topology.py

The topology.py module implements the Topology class. It is responsible for tracking and deploying the logical topology for executing scripts by configuring PDU outlets and enabling/disabling DUT ports.

#### tshark.py

This module packages the "pyshark" module which is Python's interface to the  tshark commandline tool for capturing and analyzing packet contents. It contains 2 methods for capturing and filtering packets: the first captures packets and stores them to an output file for later analysis, and the second captures packets by listening to a network interface in real-time.

#### ui_script.py

This module contains the two primary base classes for the ATLAS framework: User-Interface (UI) and Script.

##### User-Interface Class

The UI class is the base class for all API libraries requiring an interface between the workstation and another device, such as a DUT, TG host, PDU, remote servers, or another workstation. It is also the base class if the library API needs to interface with the workstation itself, such as executing a command in the terminal or operating a Tcl interpreter. Available interface types include Command Line Interface (CLI such as console serial port, Telnet, SSh, Bash) and Simple Network Management Protocol (SNMP). The UI class also contains methods for printing and formatting log messages originating from all active interfaces. Since the program can create multiple UI instances, it is possible to create more than one interface for several devices and operate them simultaneously within a single test script.

##### Script Class

The Script class is the base class for all test case scripts. It implements the following methods which handles common tasks to be performed during execution of each script:
* initUI - Initializes an UI object based on device and user-interface type.
* closeAllUI - terminates all UI objects that are previously initialized.
* beginLog - Creates a log stream and starts logging the script output.
* endLog - closes the log stream and renames the file based on test result PASS, FAIL, CHECK, or ERROR. A test result of CHECK means that there were no PASS or FAIL conditions recorded by any criteria in the log, and ERROR means there are critical failures or syntax mistakes preventing complete execution of the script.

### io

This is the default directory for storing files transfered between the workstation, TG host, and DUT. It contains a "sonic" sub-directory which stores default config_db.json files for various models. The automatically generated "tg_config.tcl" configuration file is created at the io root for copying to the TG host. If your script or library requires file transfer between devices, you should also create the file here.

### lib

This directory stores all the API libraries for controlling the DUT, TG host, PDU, and the workstation. APIs are organized into sub-directories according to the platform, model, and user-interface type. All library classes inherit the UI base class from the core package.

#### Lib Example 1 - CLI_sys_mgmt.py

This is an example library using the command line interface to control general system management functions. It contains all the basic components for a standard lib module:
* Comment block
* Import statements
* Class declaration
* Constructor
* API methods
See the in-line comments for further explanation of key points for library implementation.
  ```
  #!/usr/bin/python3
  ##################################################################################################
  """
  Module Name: CLI_sys_mgmt.py
  Purpose:
    General system management functions operating on CLI interface.
  History:
    # Use yyyy/mm/dd date format inside comments.
    2020/05/15 - Michael Chen, created.
  
  Copyright(c) Accton Technology Corporation, 2019, 2020.
  """
  
  # Import all dependency modules alphabetically starting with the "import" keyword followed by
  # the "from" keyword.
  import re

  from core.ui_script import UI

  # Declare the library class inheriting from UI class.
  class CLI_SystemMgmt(UI):

    # This is the default constructor for library classes. It must take on exactly 2 arguments as
    # shown below in order for the Script.initUI method to function correctly.
    def __init__(self, dut, ui_type):
      super().__init__(dut, ui_type)

      # Put anything here that needs to be executed during initialization of this library

    # API Example 1: showVersion
    def showVersion(self):
      """
      Method Name: showVersion
      Purpose:
        Shows DUT version information.
      History:
        2020/05/15 - Michael Chen, created.
      """

      # the UI.log method prints a message in the log. It can take any number of arguments, and
      # prints each on a separate line. If the first argument is all capitalized or if it is in
      # the form "title=xxx", it prints the string surrounded by divider symbols such as "=".
      UI.log('ACTION', 'Show system version information.')

      # For UI objects operating on the CLI interface, the two basic methods for interacting with
      # the device are the "send" and "expect" methods. "send" takes a string argument as a
      # command and executes it on the device. "expect" waits for the output buffer to return a
      # string matching its argument such as the prompt string.
      prompt = self.getPrompt()
      self.send('show version\r')
      self.expect(prompt)
      UI.log('The system version is displayed above.')

    # API Example 2: chkSystemModel
    def chkSystemModel(self, log_terminal_output=True):
      """
      Method Name: chkSystemModel
      Purpose:
        Checks the system model name.
      History:
        2020/05/15 - MichaelChen, created.
      """

      prompt = self.getPrompt()
      self.log_terminal_output = log_terminal_output
      UI.log('ACTION', 'Check system model name.')
      prompt = self.getPrompt()
      self.send('show version\r')
      self.expect(prompt)

      # the getBuff method retrieves the terminal output between the last command from "send" up to
      # the expect string. This is useful for matching criteria strings in the buffer to determine
      # pass/fail conditions.
      buff = self.getBuff()
      match = re.search('^.*' + self.model, buff, re.M)
    
      if match:
        UI.log('PASS', 'The model name is found.', match.group(0))
      else:
        UI.log('FAIL', 'The model name "%s" is not found.' %self.model)
  ```

#### Lib Example 2 - CLI_ACL.py

This is an example from the lib.sonic package. For the SONiC platform, majority of system configurations requires editing the config_db.json database file. The "configACLTable" API shows how to do this using the core.config package.
  ```
  #!/usr/bin/python3
  ##################################################################################################
  """
  Module Name: CLI_ACL.py
  Purpose:
    Access     control list methods operating on CLI interface.
  History:
    2020/02/26 - Michael Chen, created.

  Copyright(c) Accton Technology Corporation, 2019, 2020.
  """

  import re

  from core.config import setJsonConfig
  """
  Function Name: setJsonConfig
  Purpose:
    Set json variable with value using recursion.
  Input:
    json_dict - Initial dict
    key_list - The list of keys specifying the config variable
    value - Value of key to set
    action - "add" key and value or "del" key
  History:
    2020/03/04 - Albert Peng, created.
  """

  from core.ui_script import UI
  from lib.sonic.CLI_Common import CLI_Common

  # It is possible to inherit from a child of the UI class instead of UI itself. This allows
  # methods to use other methods declared in the child class. In this example, methods inside
  # CLI_ACL is able to use methods declared in CLI_Common.
  class CLI_ACL(CLI_Common):
    def __init__(self, DUT, ui_type):
      super().__init__(DUT, ui_type)

    def configACLTable(self, name, stg='', type='', ports='', desc='', delete=False):
      """
      Method Name: configACLTable
      Purpose:
        Configures ACL table entry.

      # For methods taking on multiple arguments which may not be self-explanatory, it is
      # customary to provide a more detailed description of each input to assist other developers
      # with its usage.
      Input:
        name - name of table.
        stg - stage; "INGRESS" or "EGRESS".
        type - table type.
        ports - table port binding.
        desc - description of policy
        delete - delete table
      History:
        2020/03/23 - Michael Chen, created.
      """

      if delete:
        # If "null" is required for the value of a json key, set the dict value to "None".
        setJsonConfig(UI.SONIC_CONF[self.id], ['ACL_TABLE', name], None)
      else:
        if stg != '':
          setJsonConfig(UI.SONIC_CONF[self.id], ['ACL_TABLE', name, 'stage'], stg)

        if type != '':
          setJsonConfig(UI.SONIC_CONF[self.id], ['ACL_TABLE', name, 'type'], type)

        if ports != '':
          setJsonConfig(UI.SONIC_CONF[self.id], ['ACL_TABLE', name, 'ports'], ports)

        if desc != '':
          setJsonConfig(UI.SONIC_CONF[self.id], ['ACL_TABLE', name, 'policy_desc'], desc)

  # Truncated lib content.
  ```

### scripts

The scripts directory contains test case script modules organized by platform, topology, and test design. The standard components of a script module are the following:
* Comment block
* import statements
* Class declaration
* Constructor
* "deployTopology" method
* "run" method
* "stop" method

#### Script Example 1 - IPv6_ACL_0010.py

The following example code shows the first three steps  of a script from the sonic.T1.IPv6_ACL package. It demenstrates how to create workstation, DUT, and TG UI objects and invoking their instance methods for accomplishing test procedures. The standard workflow for initializing, configuring and updating/reloading the SONiC config_db.json file is presented. This example also shows how to send packets with the traffic generator.
  ```
  #!/usr/bin/python3
  ##################################################################################################
  """
  Module Name: IPv6_ACL_0010.py
  Headline:
    Match SRC IPv6 Address Rule Test
  Purpose:
    To verify ACL can work correctly when setting match rule Src-IPv6 address.
  History:
    2020/03/31 - Michael Chen, created.

  Copyright(c) Accton Technology Corporation, 2019, 2020.
  """

  from core.topology import Topology
  from core.ui_script import Script, UI
  from lib.consts import *

  # All TC script classes inherit from the Script class in the core package. The constructor must
  # also match exactly as shown below.
  class IPv6_ACL_0010(Script):
    def __init__(self):
      super().__init__(script_path=__file__, script_doc=__doc__)

    # This method is invoked first in the main loop to deploy the test case topology.
    def deployTopology(self):
      topo = Topology('T1')
      topo.deploy()

    # The "run" method constitutes the body of the script. Any exception from the execution of
    # "run" is caught in the main loop and handled gracefully. So, you do not need to worry about
    # error handling in your script and library.
    def run(self):
      # IPv6 address local constants
      ipv6_1 = '3ffe:1:2:3:4::1234/128'
      ipv6_2 = '4ffe:1:2:3:4::1234/128'
      ipv6_3 = '3ffe:1:2:3:4::1235/128'
      ipv6_4 = '4ffe:1:2:3:4::1235/128'

      # Define a local copy of the UI.DUT1 dict to save some typing.
      DUT1 = UI.DUT1

      # When an UI object is initialized with the Script.initUI method, it combines all library
      # classes passed in as its argument into an aggregated class containing all methods from
      # each library class. For example, the following line creates a workstation object "ws"
      # with the Bash interface which has access to all methods in both "Utility" and "FileMgmt"
      # libraries.
      ws = self.initUI(UI.WS, 'bash', 'Utility', 'FileMgmt')
      tg = self.initUI(UI.TG, 'tcl', 'TrafficGen')
      dut1 = self.initUI(DUT1, DUT1['ui_type'], 'ACL', 'VLAN')

      dut1.rebootDUT()
      dut1.waitingReboot(show_log=False)

      # Initialize the config_db.json dict object for configuring SONiC device settings.
      ws.initConfigDB(dut1)
      dut1.setPortIP(DUT1['mgmt'], DUT1['ip'])

      UI.log('Clear DUTPA~C IP and join ports to VLAN10.')
      dut1.setPortIP([DUT1['PA'], DUT1['PB'], DUT1['PC']], action='clear')
      dut1.setVlan(10, [DUT1['PA'], DUT1['PB'], DUT1['PC']])
      dut1.setVlanMember(10, [DUT1['PA'], DUT1['PB'], DUT1['PC']], 'untag')

      # For procedures and criteria, print using the keywords "step" and "criteria" in the first
      # argument to "UI.log". If execution of the step encounters an error or failure, the
      # information is printed in the test report.
      UI.log('Step 1',
          'Create one ingress IPv6 ACL table 1 that has 2 rules to bind DUT1PA.',
          'r1. Priority is 10, src-ipv6 address is %s, action is drop.' %ipv6_1,
          'r2. Priority is 20, src-ipv6 address is %s, action is drop.' %ipv6_2)
      UI.log('Criteria 1',
          'ACL table and rule shall be created and show by show acl tbl and show acl rule command.')

      # Invoke the "configACLTable" method from lib example 2.
      dut1.configACLTable(name=1, stg='INGRESS', type='l3v6', ports=DUT1['PA'],
          desc='IPv6_ACL_0010-1')

      dut1.configACLRule(tbl=1, rule=1, prio=10, act='drop', type='ipv6any', src_ipv6=ipv6_1)
      dut1.configACLRule(tbl=1, rule=2, prio=20, act='drop', type='ipv6any', src_ipv6=ipv6_2)

      # Export and upload the config_db.json file from the workstation onto the DUT and reload the
      # configuration using CLI command.
      ws.exportConfigDB(dut1)
      ws.updateDevice('reload', dut1)

      dut1.chkACLTable(name=1, type='l3v6', desc='IPv6_ACL_0010-1')
      dut1.chkACLRule(tbl=1, rule=1, prio=10, act='drop', type='ipv6any', src_ipv6=ipv6_1)
      dut1.chkACLRule(tbl=1, rule=2, prio=20, act='drop', type='ipv6any', src_ipv6=ipv6_2)

      UI.log('Step 2',
          'TG2 to send learning packets.')

      # Initialize the traffic generator for testing.
      tg.initPort([1, 2, 3])
      tg.editStream(1, src_mac=MAC_1, dest_mac=BROADCAST_MAC)
      tg.editStream(2, src_mac=MAC_2, dest_mac=BROADCAST_MAC)
      tg.editStream(3, src_mac=MAC_3, dest_mac=BROADCAST_MAC)
      tg.macLearning()

      UI.log('Step 3',
          'From TG1 to input %s packets that DA is TG2 and src-ipv6address is %s.'\
          %(TRMC_100K, ipv6_1))
      UI.log('Criteria 3',
          'TG2 and TG3 shall not receive %s packets.' %TRMC_100K)

      # The following sequence of code shows the general workflow of sending traffic using the
      # traffic generator.
      tg.editStream(1, burst=TRMC_100K, src_mac=MAC_1, dest_mac=MAC_2, src_ipv6=ipv6_1,
          dest_ipv6=IPv6_MULTICAST)

      tg.clearCounter([1, 2, 3])
      tg.runPackets(1)
      tg.detectSendComplete(1, packets_list=TRMC_100K)
      tg.showCounter([1, 2, 3])
      tg.chkPacketCount([2, 3], 0, get_type='RxFrames', error_range=10)

    # ... Truncated script content...

    # The "stop" method is invoked after execution of "run". This portion of the script is
    # executed reguardless of whether "run" encounters an exception.
    def stop(self):
      UI.log('Close all interfaces.')

      # Find the UI object for the traffic generator and release the previously reserved ports.
      for ui in self.ui_list:
        if hasattr(ui, 'releasePort'):
          ui.releasePort([1, 2, 3])

      # Terminate all opened user interfaces.
      self.closeAllUI()
  ```

#### Script Example 2 - VLAN_Trunk_0060.py

This example from sonic.T1.VLAN_Trunk demonstrates how to capture packets with tshark filters using the traffic generator.
  ```
  #!/usr/bin/python3
  ##################################################################################################
  """
  Module Name: VLAN_Trunk_0060.py
  Headline:
    VLAN Trunk - Receiving Single Tagged Packet
  purpose:
    To verify single tagged packet received is processing correctly according to the VLAN settings
    on ingress and egress ports. Ingress single tagged packet will be dropped if VLAN ID of the
    packet is not in permit VLAN IDs of ingress port. If VLAN ID of the packet is equal to untagged
    VLAN ID of egress port, the packet will be send out untagged. If VLAN ID of the packet is equal
    to tagged VLAN ID of egress port, the packet will be send out tagged.
  History:
    2020/04/20 - Albert Peng, Create

  Copyright(c) Accton Technology Corporation, 2019, 2020.
  """

  from lib.utils import *
  from core.topology import Topology
  from core.ui_script import Script, UI

  class VLAN_Trunk_0060(Script):
    def __init__(self):
      super().__init__(__file__, __doc__)

    def deployTopology(self):
      topo = Topology('T1')
      topo.deploy()

    def run(self):
      DUT1 = UI.DUT1
      tg = self.initUI(UI.TG, 'tcl', 'TrafficGen')
      ws = self.initUI(UI.WS, 'bash', 'Utility', 'FileMgmt')
      dut1 = self.initUI(UI.DUT1, UI.DUT1['ui_type'], 'VLAN', 'Common')

      UI.log('STEP 1', 'Start the testing with factory setting and set NIC IP.')
      dut1.rebootDUT()
      dut1.waitingReboot()

      ws.initConfigDB(dut1)

      UI.log('STEP 2', 'Set mgmt port IP')
      dut1.setPortIP(DUT1['mgmt'], DUT1['ip'])

      UI.log('STEP 3', 'Remove the default IP address configuration on DUT ports in '\
          'config_db.json, and then reload config.')

      dut1.setPortIP([DUT1['PA'], DUT1['PB'], DUT1['PC']], action='clear')

      UI.log('STEP 4', 'Add VLAN 11.')
      dut1.setVlan(11, [DUT1['PA'], DUT1['PB'], DUT1['PC']])
      
      UI.log('STEP 5', 'Add DUT1PA as VLAN 11 untagged member.')
      dut1.setVlanMember(11, DUT1['PA'], 'untag')

      UI.log('STEP 6', 'Add DUT1PB as VLAN 11 untagged member.')
      dut1.setVlanMember(11, DUT1['PB'], 'untag')

      UI.log('STEP 7', 'Add DUT1PC as VLAN 11 untagged member.')
      dut1.setVlanMember(11, DUT1['PC'], 'tag')

      ws.exportConfigDB(dut1)
      ws.updateDevice('reload', dut1)
      
      UI.log('STEP 8', 'Inject VLAN 11 tagged packets from TG1.')
      UI.log('CRITERIA 8',
          'TG2 should receive the injected packets and they are untagged.',
          'TG3 should receive the injected packets and they are VLAN 11 tagged.')

      tg.initPort([1, 2, 3])

      tg.editStream(1, src_mac='00-00-00-00-00-01', dest_mac='FF-FF-FF-FF-FF-FF', vid=11, rate=25,
          burst=100)

      tg.triggerPacket([2, 3], '00-00-00-00-00-01')

      # ------ ↓ Check TG2 ↓ ------

      tg.clearCounter()
      tg.capturePackets(2)
      tg.runPackets(1)
      tg.detectSendComplete(1, 100)

      tg.chkPacketCount(2, 100, get_type='RxTrigger')
      tg.showCounter()
      tg.exportCapturePackets(ws)

      ws.captureFilePackets('TG2_cap_pkts.enc', 'vlan.id == 11')
      ws.chkCaptureCount(0)

      # ------ ↓ Check TG3 ↓ ------

      tg.clearCounter()
      tg.capturePackets(3)
      tg.runPackets(1)
      tg.detectSendComplete(1, 100)

      tg.chkPacketCount(3, 100, get_type='RxTrigger')
      tg.showCounter()
      tg.exportCapturePackets(ws)

      ws.captureFilePackets('TG3_cap_pkts.enc', 'vlan.id == 11')
      ws.chkCaptureCount(100)

  # ... Truncated script content...
  ```

### main.py

Finally, this is the main module of ATLAS. It serves as the entry point for the program; the main thread starts and ends inside this module. Execution of main takes several options that determine the DUT, test cases, and slave node configuration to be used. Several logging options are provided for controlling how test log messages should be presented to the user. The complete list of available options and their description is printed when running "main.py -help".

---

**ATLAS Project Readme**
**Copyright(c) Accton Technology Corporation, 2019, 2020.**
