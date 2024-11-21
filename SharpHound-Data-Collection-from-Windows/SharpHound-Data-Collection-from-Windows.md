## SharpHound - Data Collection from Windows


- [SharpHound](https://github.com/BloodHoundAD/SharpHound) is the official data collector tool for [BloodHound](https://github.com/BloodHoundAD/BloodHound), is written in C# and can be run on Windows systems with the .NET framework installed. The tool uses various techniques to gather data from Active Directory, including native Windows API functions and LDAP queries. 

- The data collected by SharpHound can be used to identify security weaknesses in an Active Directory environment to attack it or to plan for remediation. 

- This section will discover the basic functionalities of enumerating Active Directory using SharpHound and how to do it. 



### Basic Enumeration


- By default SharpHound, if run without any options, will identify the domain to which the user who ran it belongs and will execute the default collection. Let's execute SharpHound without any options. 


### Running SharpHound without any options


![bloodhound](/SharpHound-Data-Collection-from-Windows/images/sharphound.png) 


- The 2nd line in the output above indicates the collection method used by default: **Resolved Collection Methods:** 

- **Group, LocalAdmin, Session, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote.** 

- Those methods mean that we will collect the following: 

1. Users and Computers. 
2. Active Directory security group membership. 
3. Domain trusts. 
4. Abusable permission on AD objects. 
5. OU tree structure. 
6. Group Policy links
7. The most relevant AD object properties. 
8. Local groups from domain-joined Windows system and local privileges such as RDP, DCOM, and PSRemote. 
9. User sessions
10. All SPN (Service Principal Names). 


- To get the information for local groups and sessions, SharpHound will attempt to connect to each domain-joined Windows computer from the list of computers it collected. If the user from which SharpHound is running has privileges on the remote computer, it will collect the following information:: 


1. The members of the local administrators, remote desktop, distributed COM, and remote management groups. 
2. Active sessions to correlate to systems where users are interactively logged on.


- Once SharpHound terminates, by default, it will produce a zip file whose name starts with the current date and ends with BloodHound. This zip archive contains a group of JSON files:



![bloodhound](/SharpHound-Data-Collection-from-Windows/images/files.png) 


### Importing Data into BloodHound


1. Start the neo4j database service. 

	> neo4j console


#### Start neo4j


![bloodhound](/SharpHound-Data-Collection-from-Windows/images/neo4j.png) 


#### Start BloodHound

2. Start bloodhound and login with the credentials that you have set for neo4j. Default is neo4j:neo4j


	> $ bloodhound --no-sandbox


![bloodhound](/SharpHound-Data-Collection-from-Windows/images/bloodhound.png) 

3. Click the upload button on the far right, browse to the zip file, and upload it. You will see a status showing upload % completion. 


![bloodhound](/SharpHound-Data-Collection-from-Windows/images/upload-2.png) 


![bloodhound](/SharpHound-Data-Collection-from-Windows/images/data.png) 



4. Once the upload is complete, we can analyze the data. If we want to view information about the domain, we can type **Domain:INLANEFREIGHT.HTB** into the search box. This will show an icon with the domain name. If you click the icon, it will display information about the node (the domain), how many users, groups, computers, OUs, etc. 


![bloodhound](/SharpHound-Data-Collection-from-Windows/images/info.png) 


5. Now, we can start analyzing the information in the bloodhound and find the paths to our targets. 
