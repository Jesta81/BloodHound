## BloodHound.py - Data Collection from Linux 


> [SharpHound](https://github.com/BloodHoundAD/SharpHound) does not have an official data collector tool for Linux. However, Dirk-jan Mollema created [BloodHound.py](https://github.com/fox-it/BloodHound.py), a Python-based collector for BloodHound based on Impacket, to allow us to collect Active Directory information from Linux for BloodHound. 


### Installation


> We can install [BloodHound.py](https://github.com/fox-it/BloodHound.py) with **pip install bloodhound** or by cloning its repository and running **python setup.py install**. To run, it requires **impacket, ldap3, and dnspython.** The tool can be installed via pip by typing the following command: 

#### Install BloodHound.py

	$ pip install bloodhound


> To install it from the source, we can clone the [BloodHound.py GitHub repository](https://github.com/fox-it/BloodHound.py) and run the following command: 


#### Install BloodHound.py from the source

	$ git clone https://github.com/fox-it/BloodHound.py -q
	$ cd BloodHound.py/
	$ sudo python3 setup.py install


### BloodHound.py Options


> We can use **--help to list all BloodHound.py options**. The following list corresponds to version 1.6.0: 


#### BloodHound.py options 


![bloodhound](/BloodHound.py-Data-Collection-from-Linux/images/options.png) 

![bloodhound](/BloodHound.py-Data-Collection-from-Linux/images/options-2.png) 

![bloodhound](/BloodHound.py-Data-Collection-from-Linux/images/options-3.png) 


> As BloodHound.py uses **impacket**, options such as the use of hashes, ccachefiles, aeskeys, kerberos, among others, are also available for BloodHound.py. 


### Using BloodHound.py


> To use BloodHound.py in Linux, we will need **--domain and --collectionmethod options and the authentication method.** Authentication can be a username and password, an NTLM hash, an AES key, or a ccache file. BloodHound.py will try to use the Kerberos authentication method by default, and if it fails, it will fall back to NTLM. 

> Another critical piece is the domain name resolution. If our DNS server is not the domain DNS server, we can use the option **--nameserver, which allows us to specify an alternative name server for queries.** 



#### Running BloodHound.py


![bloodhound](/BloodHound.py-Data-Collection-from-Linux/images/usage.png) 

	$ bloodhound-python -d inlanefreight.htb -c DCOnly -u htb-student -p HTBRocks! -ns 10.129.204.226 -k


> **Note:** Kerberos authentication requires the host to resolve the domain FQDN. This means that the option --nameserver is not enough for Kerberos authentication because our host needs to resolve the DNS name KDC for Kerberos to work. If we want to use Kerberos authentication, we need to set the DNS Server to the target machine or configure the DNS entry in our hosts' file. 


- Let's add the DNS entry in our hosts file: 

![bloodhound](/BloodHound.py-Data-Collection-from-Linux/images/hosts.png) 


> Use BloodHound.py with Kerberos authentication: 

### Using BloodHound.py with Kerberos authentication 


![bloodhound](/BloodHound.py-Data-Collection-from-Linux/images/kerberos.png) 


> **Note:** Kerberos Authentication is the default authentication method for Windows. Using this method instead of NTLM makes our traffic look more normal. 


### BloodHound.py Output files

> Once the collection finishes, it will produce the JSON files, but by default, it won't zip the content as SharpHound does. If we want the content to be placed in a zip file, we need to use the option **--zip.** 


#### List All Collections of BloodHound.py 


![bloodhound](/BloodHound.py-Data-Collection-from-Linux/images/files.png) 


### Using the Data 


> Now that we have seen various ways to ingest data for the **INLANEFREIGHT.HTB domain** and import it into the **BloodHound GUI** let's analyze the results and look for some common issues. 
