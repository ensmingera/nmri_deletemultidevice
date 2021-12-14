# nmri_deletemultidevice.py

This script will delete multiple devices from NetMRI by reading a list of DeviceID and then sending the request via API.

It is designed to run from the NetMRI sandbox, but you can run this script from a machine that contains the below prerequisites.


### Prerequisites
  - Python 3.5+
  - Infoblox NetMRI Python API client package (infoblox_netmri)

### Usage
```
nmri_deletemultidevice.py [-h] -l LIST [-n HOST] [-u USER] [--unitid UNITID]
```

**Arguments**:
- **-h, --help**
  -  Display the help messsage
- **-l LIST, --list LIST** 
  - *(Required)* Filename that contains the list of DeviceIDs to delete. 
- **-n HOST, --host HOST**
  - The hostname or IP address of NetMRI. *(Default: netmri)* 
- **-u USER, --user USER**
  - Username to authenticate to NetMRI. *(Default: admin)*
- **--unitid UNITID**
  - UnitID of collector to send delete request to (used in OC environment).

### Creating a list of Device ID's
The easiest method to create a list of Device ID's is to CSV export from the ```Network Explorer > Discovery``` page.  
1. Create filters if necessary and then click on the button for ```CSV Export```.  
2. After opening the CSV, copy the values from the ```DeviceID``` column to another file.  
If the NetMRI is an Operations Center, take note of the value from the ```UnitID``` column. You will have to specify this number with the ```--unitid``` argument.
  
The list should just contain the values from the DeviceID column:
```
[root@sandbox ~]# cat list.txt
89444703861009224837
54819848293127573233
40568721994384821112
[root@sandbox ~]# 
```

***If using Microsoft Excel to open the CSV:***  
Be aware that if you are using Microsoft Excel to open the CSV, that the values in the DeviceID column will not be accurate. This is a result of Excel attempting to auto-format the DeviceID column to a *Numbers* column.  
  
To avoid this, follow these steps:
1. Change the extension of the CSV file to ```.txt```.
2. Open the file in Excel.
3. The *Text Import Wizard* will appear. Select the option for ```Delimited```. Then click Next.
4. Check mark the box for ```Comma``` for the delimiters. Then click Next.
5. Select the ```DeviceID``` column, and then select the option for ```Text```. Then click Finish.
6. The values in the DeviceID column will now be accurate.


### Examples
**Running from a standalone NetMRI sandbox:**  
*Send deletion request for DeviceIDs from filename ```list.txt```*
```
./nmri_deletemultidevice.py -l list.txt
```
  
**Running from an NetMRI Operations Center sandbox:**  
*Send deletion request for DeviceIDs from filename ```list.txt``` on UnitID (collector ID) ```5```*
```
./nmri_deletemultidevice.py -l list.txt --unitid 5
```
  
**Running from an external machine to a standalone NetMRI:**  
*Send deletion request for DeviceIDs from filename ```list.txt```, to a standalone NetMRI at ```10.200.100.75```, as username ```aensminger```.*
```
./nmri_deletemultidevice.py -l list.txt -n 10.200.100.75 -u aensminger
```
  
**Running from an external machine to a NetMRI Operations Center:**  
*Send deletion request for DeviceIDs from filename ```list.txt```, to an Operations Center at ```172.16.100.1```, as username ```aensminger```, specifying UnitID (collector ID) ```2```.*
```
./nmri_deletemultidevice.py -l list.txt -n 172.16.100.1 -u aensminger --unitid 2
```
