# EdgeAutoDocumentation
#### By Dante Gennero
This is the documentation for the Virtual Machine portion of the project EdgeAuto.

##### Table of Contents

[Google Cloud Setup](/README.md#vm-setup-on-google-cloud)

[Nodes](/README.md#nodes)

[DataCenter](/README.md#data-center)

[Initial VM Setup](/README.md#initial-setup-on-vms)

[Dependencies](/README.md#dependencies)

[License](/README.md#license)

# VM setup on Google Cloud

Go to [Google Cloud Console](console.cloud.google.com) and login. 

### Firewall Rules

Navigate to VPC Network and select Firewall Rules

We are creating 2 Firewall Rules.
One for incoming connections and one for outgoing.

#### Incoming Firewall Rule:
1. Name it something memorable, we used TCPIN
2. Direction of traffic: Ingress
3. Target Tags: edgeauto
4. Source IP Ranges: 0.0.0.0/0 This is used for all ranges.
5. Protocols and Ports: Allow all or the specific ports you are using. Check TCP and put the port you want to use. We used 8001.

#### Outgoing Firewall Rule:
1. Name it something memorable, we used TCPOUT
2. Direction of traffic: Egress
3. Target Tags: edgeauto
4. Source IP Ranges: 0.0.0.0/0
5. Protocols and Ports: Allow all or the specific ports you are using. Check TCP and put the port you want to use. We used 8002.



### Nodes
Navigate to the compute engine and on the left Click Instance Templates.

1. Name your Instance: We used the scheme edgeauto-#
2. Select  machine type: 1vCPU with 3.75GB of memory
3. Select Boot Disk: Ubuntu 16.04 LTS
4. Allow HTTP Traffic and HTTPS Traffic
5. Select Networking and give the template a Network Tag. We used edgeauto.

Create the template. Now when you want to create a node you can use that template by clicking create instance and then creating it with the template we just made.


### Data Center
For our DataCenter:
1. Machine Type: 2vCPUs
2. Boot Disk: Ubuntu 16.04 LTS
3. Allow HTTP/HTTPS Traffic
4. Add a your Network Tag. Ours was edgeauto.

Create the VM

# Initial Setup on VMs
Install git if you don't have it 

`sudo apt-get install git`

Pull from my github

`git clone https://github.com/SirAlfy/EdgeAutoDocs.git`

Navigate to that folder

`cd EdgeAutoDocs`

Run the bash file to start installing the dependencies

`sh edgeautoSetup.sh`

Open VMInfo and put in all the information you have. Internal IP of this VM, port and the datacenter ip and its port.

After installation you are able to run the socket. I would recommend doing this in a screen

`screen -S PhpSocket`

followed by running the socket. Node for edge nodes and Datacenter for the DataCenter.

`php nodeSocket.php` or `php datacenterSocket.php` 

To send the data to our Datacenter run the following command to execute it once

`php writeToDC.php`

or you can schedule it by editing your crontab

`crontab -e`

Then put the following in with the following format

`Minute(0-59) Hour(0-23) Day(1-31) Month(1-12) Day of Week (0-6)(0=Sunday) /path/to/yourShellScript`

We set our VMs to stagger their writing to the datacenter every minute.

VM1 `0,5,10,15,20,25,30,35,40,45,50,55 * * * * php /home/*YOUR USERNAME*/EdgeAutoDocs/writeToDC.php`  

This will run every 5 minutes. Ex: 1:00pm, 1:05pm...

VM2 `1,6,11,16,21,26,31,36,41,46,51,56 * * * * php /home/*YOUR USERNAME*/EdgeAutoDocs/writeToDC.php` 

VM3 `2,7,12,17,22,27,32,37,42,47,52,57 * * * * php /home/*YOUR USERNAME*/EdgeAutoDocs/writeToDC.php` 

VM4 `3,8,13,18,23,28,33,38,43,48,53,58 * * * * php /home/*YOUR USERNAME*/EdgeAutoDocs/writeToDC.php` 



# Dependencies
Apache : https://httpd.apache.org/download.cgi#apache24

Mysql : https://dev.mysql.com/downloads/file/?id=482330

Php-Mysql : https://dev.mysql.com/downloads/repo/apt/

Php : https://www.php.net/distributions/php-7.0.33.tar.gz

# License

You are free to use this code.
