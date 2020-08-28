## Table of Contents

* [Config File Structure](#config-file-structure)
* [Setup JSON File Editor](#setup-json-file-editor)
* [Config File](#config-file)
  * [PDU](#pdu)
  * [TG](#tg)
  * [DUT_model](#dut_model)
  * [DUT1, DUT2, DUT3](#dut1-dut2-dut3)
  * [topology](#topology)
  * [feature](#feature)

## Config File Structure
The ATAS is use json as config file, the core file include `PDU`, `TG`, `workstation`, `DUT_model`, `DUT1`, `DUT2`, `DUT3` and `topology`.

```markdown
+ config
  - DUT_model
  - DUT1
  - DUT2
  - DUT3
  - PDU
  - TG
  - workstation
  + [ PLATFORM ]
    - topology
    - feature
    + [ MODEL ]
      - feature
```

## Install JSON File Editor

1. Downaload and install [notpad++](https://notepad-plus-plus.org/downloads/)
2. Install `JSON Viewer` plugin.
>     Run Notepad++ > Plugin > Plugins Admin > Install JSON
>>     JSON Viewer > Show JSON Viewer: Will create json format tree view window
>>     JSON Viewer > Format JSON: Will auto arrange file content to json tree view

## Config File

#### PDU
The [PDU.json](../../blob/master/src/config/PDU.json) is setting PDU vairable that include `brand`, `ip` and `index(O1 ~ O12)` mapping `outlet`.

Item           | Option          | Description
:--------------|:----------------|:-----------
pdu_brand      | ATEN / Foxconn  | Using brand of PDU
ip             |                 | PDU IP 
O1 ~ O12       | 1 ~ 12          | PDU outlet index

#### TG
The [TG.json](../../blob/master/src/config/TG.json) is setting TG varaible that include `brand`, `ip`, `speed`, `version` and `index(1 ~ 12)` mapping `port`.

Item           | Option                  | Description
:--------------|:------------------------|:-----------
tg_brand       | IXIA / STC              | Using brand of TG
ip             |                         | TG IP 
speed          | 1G ~ 100G               | TG speed
version        |                         | TG version
auto_nego      | Enable / Disable        | auto negotiation
fec_status     | Enable / Disable        | 
fec_type       | rs, base-r, none        | 
1 ~ 12         | [ Chassis, Slot, Port ] | TG port mapping TG index for script

#### DUT_model
The [DUT_model.json](../../blob/master/src/config/simba/DUT_model.json) is setting seldom change DUT value.

Item           | Option          | Description
:--------------|:----------------|:-----------
platform       |                 | DUT platform
port_count     |                 | DUT port count
reboot_time    | 120             | 
speed          |                 | DUT using speed
baud_rate      | 115200 / 9600   | 
ssh_port       | 22              | 
telnet_port    | 23              | 
username       |                 | Deafult usernaem          
password       |                 | Deafult password 
prompt         | #               | 
community      | private         | SNMP community
snmp_version   | 2c              | SNMP version

#### DUT1, DUT2, DUT3
The [DUT1](../../blob/master/src/config/simba/DUT1.json), [DUT2](../../blob/master/src/config/simba/DUT2.json), [DUT3](../../blob/master/src/config/simba/DUT3.json) is setting DUT using `UI`, `IP` and `DUT port` mapping `index`.

Item                  | Option                                  | Description
:---------------------|:----------------------------------------|:-----------
ui_type               | console / telnet / ssh / console_server | DUT connection type
com_port              | COM1 ~ N                            　  | DUT console 
console_server_ip     |                                     　  | Console server IP
console_server_port   |                                     　  | Console server port
ip                    |                                         | DUT IP
mac                   |                                         | DUT MAC address
outlet                | O1 ~ O12                                | DUT using PDU outlet
PA ~ PL               |                                         | DUT port name

#### topology
The [topology.json](../../blob/master/src/config/simba/topology.json) is control `DUT port` link `up/down` and `PDU outlet` power `on/off`.

Item           | Option          | Description
:--------------|:----------------|:-----------
PA ~ PL        | up / down       | DUT port
DUT1_outlet    | up / down       | The PDU outlet connect DUT1
DUT2_outlet    | up / down       | The PDU outlet connect DUT2
DUT3_outlet    | up / down       | The PDU outlet connect DUT3

#### feature
It's standard format, if create new feature config file, please follow example format, the [ MODEL ] feature config will cover same item. (example: [ratelimit](../../blob/master/src/config/simba/rate_limit.json))

Item           | Option          | Description
:--------------|:----------------|:-----------
capture_count  | 30              | Capture packets count
formula        | 1 / 2           | Calculation formula
__formula      |                 | __ begining is description
