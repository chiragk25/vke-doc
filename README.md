# vke v0.9 (Build: 0ef237d)

Command line interface for VMware Kubernetes Engine
## List of supported commands

### 1 ```account```
Commands to log in and show account info


#### 1.1 ```account login```
Log in to your organization

Description: 

To see your organization ID, log in to the VKE console, click on your name/org
	at the top of the page, and then click on the abbreviated organization ID to see
	the full organization ID. To get your refresh token, click on your name/org at the
 	top of the page, click My Account, and then click API Tokens. 

	Example: 
       vke account login -t fd2c1d78-9f00-4e30-8268-4ab8162080d \
                     -r 5fde5099-f329-4f1a-a580-fe359d919a7

Flags: 
```
--organization value, -t value	specifies the VMware cloud services organization
--refresh-token value, -r value	OAuth refresh token obtained from https://console.cloud.vmware.com
--target value	sets the target
--no-cert-check, -c	flag to avoid validating the server's certificate
--proxyurl value, -p value	sets the proxy url for http client
```
#### 1.2 ```account show```
Show current logged in user info


### 2 ```cluster```
Commands to manage clusters


#### 2.1 ```cluster auth```
Sub-commands to manage kubectl authentication


##### 2.1.1 ```cluster auth delete```
Remove the configuration from kubectl config file for your cluster

Description: 

Remove the entries from your default kubectl configuration file for your cluster 

    Example: 
      vke cluster auth delete TestCluster

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.1.2 ```cluster auth setup```
Configure kubectl to allow communication with your cluster

Description: 

Modify your default kubectl configuration file to allow access to your cluster 

    Example: 
      vke cluster auth setup TestCluster

Flags: 
```
--username value, -u value	Username for login
--password value, -p value	Password for login
--idp value, -i value	specifies the identity provider; supported providers are csp (default) and lightwave
--embed-ca, -e	Embed CA cert into kubectl config
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.2 ```cluster create```
Create a new cluster

Description: 

Create a new Kubernetes cluster. 

    Example: 
     vke cluster create -n TestCluster -v 1.7.5 --region us-west-2 

Flags: 
```
--name value, -n value	Cluster name
--service-level value, -l value	DEVELOPER: No HA for etcd or masters. ENTERPRISE: etcd and masters 
		are deployed in HA fashion (multiple nodes & AZs)
--display-name value, -d value	The friendly name of the cluster which is used for display
--network-tenancy value	SHARED: Clusters will share Tenant's network in the region. 
DEDICATED: Clusters will have their own networks
--cluster-network value	CIDR representation of the cluster network (optional). 
		If not specified, the default value is '10.1.0.0/16'
--pod-network value	CIDR representation of the pod network (optional). 
		If not specified, the default value is '10.2.0.0/16'
--service-network value	CIDR representation of the service network (optional). 
		If not specified, the default value is '10.0.0.0/24'
--region value, -r value	Region where cluster should be created
--version value, -v value	Kubernetes version that should be created
--template value, -t value	Kubernetes smart cluster template name
--privilegedMode	Creates a cluster with privileged mode capabilities.
		What is privileged mode?
		A collection of root-equivalent functionalities available for all pods running 
		on the cluster. Privileged mode includes the following Kubernetes features: 
		hostPath, hostPID, hostNetwork, hostIPC, privileged pods and all linux capabilities.
		WARNING: By selecting "privileged mode", users enable a set of root-equivalent 
		functionalities for pods running on the cluster. Applications with privileged
		mode enabled have two risks. First, an application may modify the underlying OS 
		in ways that may result in an unstable node or cluster and ultimately result in 
		the inability for the VKE service to fully manage the smart cluster. Second, if  
		an application is compromised by a malicious attacker, it is easier to spread 
		the attack to other applications in the same cluster.
--force	This option will force creating a cluster with privileged mode capabilities.
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.3 ```cluster delete```
Delete a cluster

Description: 

Delete a cluster. WARNING: All running applications will be forcibly stopped. 

    Example: 
      vke cluster delete TestCluster

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.4 ```cluster get-kubectl-auth```
Generate the kubectl command for authentication

Description: 

Generate the kubectl (Kubernetes command line client) configuration. There are two options. 
   1. Run vke cluster get-kubectl-auth TestCluster with --configfile option 
   to create a new Kubernetes config. You can move the created file to ~/.kube/config (Mac OS X and Linux) to replace 
   your default config. 
   2. Run vke cluster get-kubectl-auth TestCluster without the --configfile 
   option. This will print commands that you can copy and paste to modify your Kubernetes configuration. 

    Example: 
      vke cluster get-kubectl-auth TestCluster -u user1@org-id -p password -cf config-file.txt

Flags: 
```
--username value, -u value	Username for login
--configfile value, --cf value	specifies the filename in which the user wants the kubectl configuration to be generated
--password value, -p value	Password for login
--idp value, -i value	specifies the identity provider; supported providers are csp (default) and lightwave
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.5 ```cluster iam```
Sub-commands to manage access policies


##### 2.5.1 ```cluster iam add```
Bind an identity to a role to grant permissions

Description: 

Bind an identity to a role on a cluster to grant permissions. Role can be 'smartcluster.admin', 
   'smartcluster.edit' or 'smartcluster.view'

    Example: 
       vke cluster iam add TestCluster -s user1@account.local \
             -r smartcluster.edit
       vke cluster iam add TestCluster -s group1 -r smartcluster.view

Flags: 
```
--subject value, -s value	name of the identity to bind (user or group)
--role value, -r value	name of the role to bind
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.5.2 ```cluster iam export```
Export the direct access policy to a file

Description: 


    Example: 
       vke cluster iam export TestCluster -o file.txt 

Flags: 
```
--output value, -o value	Output file path
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.5.3 ```cluster iam import```
Import an access policy from a file

Description: 


    Example: 
       vke cluster iam import TestCluster -i file.txt 

Flags: 
```
--input value, -i value	Input file path
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.5.4 ```cluster iam remove```
Remove an identity from a role binding

Description: 

Remove an identity from a role binding on a cluster. Role can be 'smartcluster.admin',
   'smartcluster.edit', 'smartcluster.view' or use '*' to remove all existing roles.

    Example: 
       vke cluster iam remove TestCluster -s user1@account.local \
              -r smartcluster.edit 
       vke cluster iam remove TestCluster -s group1 -r smartcluster.view

Flags: 
```
--subject value, -s value	name of the identity to bind (user or group)
--role value, -r value	name of the role to bind
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.5.5 ```cluster iam show```
Show the direct and inherited access policies for the cluster

Description: 


    Example: 
       vke cluster iam show TestCluster 

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.6 ```cluster list```
List clusters

Description: 

List all clusters in the current tenant. 

    Example: 
      vke cluster list

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.7 ```cluster maintain```
Start cluster maintenance

Description: 

Performs maintenance on the cluster, including re-creation of failed master nodes. 
   Please note that this should normally not be needed, except in unusual situations, 
   because maintenance is automatically performed. 

    Example: 
      vke cluster maintain TestCluster

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.8 ```cluster merge-kubectl-auth```
Configure kubectl for authentication

Description: 

Modify your default kubectl configuration file to allow access to your cluster 

    Example: 
      vke cluster merge-kubectl-auth TestCluster

Flags: 
```
--username value, -u value	Username for login
--password value, -p value	Password for login
--idp value, -i value	specifies the identity provider; supported providers are csp (default) and lightwave
--embed-ca, -e	Embed CA cert into kubectl config
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.9 ```cluster namespace```
Commands to manage namespaces


##### 2.9.1 ```cluster namespace create```
Create new namespace

Description: 


    Example:
       vke namespace create cluster1 namespace1

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.9.2 ```cluster namespace delete```
Delete a namespace

Description: 

Delete a namespace with specified name

    Example:
       vke namespace delete cluster1 namespace1

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.9.3 ```cluster namespace iam```
Sub-commands to manage access policies


###### 2.9.3.1 ```cluster namespace iam add```
Bind an identity to a role to grant permissions

Description: 

Bind an identity to a role on a namespace to grant permissions. Role can be 'namespace.admin', 
   'namespace.edit' or 'namespace.view'

    Example: 
       vke namespace iam add <cluster-name> <namespace> \
       -s user1@account.local -r namespace.edit
       vke namespace iam add <cluster-name> <namespace> \
       -s group1 -r namespace.view

Flags: 
```
--subject value, -s value	name of the identity to bind (user or group)
--role value, -r value	name of the role to bind
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
###### 2.9.3.2 ```cluster namespace iam export```
Export the direct access policy to a file

Description: 


    Example: 
       vke namespace iam export cluster1 namespace1 -o file.txt 

Flags: 
```
--output value, -o value	Output file path
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
###### 2.9.3.3 ```cluster namespace iam import```
Import an access policy from a file

Description: 


    Example: 
       vke namespace iam import cluster1 namespace1 -i file.txt 

Flags: 
```
--input value, -i value	Input file path
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
###### 2.9.3.4 ```cluster namespace iam remove```
Remove an identity from a role binding

Description: 

Remove an identity from a role binding on a namespace. Role can be 'namespace.admin', 
   'namespace.edit', 'namespace.view' or use '*' to remove all existing roles

    Example: 
       vke namespace iam remove <cluster-name> <namespace> \
       -s user1@account.local -r namespace.edit 
       vke namespace iam remove <cluster-name> <namespace> \
       -s group1 -r namespace.view

Flags: 
```
--subject value, -s value	name of the identity to bind (user or group)
--role value, -r value	name of the role to bind
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
###### 2.9.3.5 ```cluster namespace iam show```
Show the direct and inherited access policies for the namespace

Description: 


    Example: 
       vke namespace iam show cluster1 namespace1 

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.9.4 ```cluster namespace list```
List all namespaces


Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.9.5 ```cluster namespace show```
Show information about a namespace

Description: 

Shows details related to a namespace

    Example:
       vke namespace show cluster1 namespace1

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.10 ```cluster peering```
Commands to manage peerings


##### 2.10.1 ```cluster peering create```
Create new peering

Description: 

Create new peering. 

    Example:
       vke cluster peering create -c <cluster-name> -n <peering-name> \
      --customer-account-id <customer-account-id> --customer-network-id <customer-network-id>  \
      --customer-network-cidr <customer-network-cidr> --customer-network-region <network-region>

Flags: 
```
--cluster-name value, -c value	Cluster name
--name value, -n value	Peering name
--customer-account-id value	Customer account id
--customer-network-id value	Customer network id
--customer-network-cidr value	Customer network cidr
--customer-network-region value	Customer network region
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.10.2 ```cluster peering delete```
Delete a peering

Description: 

Delete a peering. 

    Example:
       vke cluster peering delete <cluster-name> <peering-id>

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.10.3 ```cluster peering list```
List all peerings for a cluster

Description: 

List all peerings of a cluster. 

    Example:
       vke cluster peering list <cluster-name>

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.10.4 ```cluster peering rename```
Rename a peering

Description: 

Update name of a peering. 

    Example:
       vke cluster peering rename <cluster-name> <peering-id> <new-peering-name>

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
##### 2.10.5 ```cluster peering show```
Show information about a peering

Description: 

Show information of a peering. 

    Example:
       vke cluster peering show <cluster-name> <peering-id>

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.11 ```cluster rename```
Change the display name of a cluster

Description: 


    Example: 
        vke cluster rename TestCluster 'New Display Name'

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.12 ```cluster show```
Show information about a cluster 

Description: 

List the cluster's name, state, type, workerCount 
   and all the extended properties. 

    Example: 
      vke cluster show TestCluster

Flags: 
```
--perf, -p	Cluster Creation Perf Report
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.13 ```cluster show-health```
Show health information about a cluster 

Description: 

List the cluster's overall health, message and health details

    Example: 
      vke cluster show-health TestCluster

Flags: 
```
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.14 ```cluster templates```
Commands to list template


##### 2.14.1 ```cluster templates list```
List Kubernetes Smart Cluster templates


#### 2.15 ```cluster upgrade```
Upgrade the Kubernetes version your cluster is running

Description: 


    Example: 
      vke cluster upgrade TestCluster -v 1.7.5

Flags: 
```
--version value, -v value	version
--upgradev2, --v2	This defines if new upgrade workflow will execute or not
--folder value, -f value	specify the current working folder
--project value, --pr value	specify the current working project
```
#### 2.16 ```cluster versions```
Commands to list Kubernetes versions


##### 2.16.1 ```cluster versions list```
List available Kubernetes versions

Description: 

Lists available Kubernetes versions for a specific region

    Example:
       vke cluster versions list --region us-west-2 

Flags: 
```
--region value, -r value	Region name (required)
```
### 3 ```documentation```
prints the CLI documentation


### 4 ```folder```
Commands to manage folders


#### 4.1 ```folder create```
Create new folder

Description: 

Create new folder

    Example:
       vke folder create folder1 --display-name 'New Folder'

Flags: 
```
--display-name value, -d value	sets the display name of the folder
```
#### 4.2 ```folder delete```
Delete a folder

Description: 

Delete a folder with specified name

    Example: 
       vke folder delete folder1 


#### 4.3 ```folder get```
Show the current working folder

Description: 

Show current folder in use for vke

    Example: 
       vke folder get 


#### 4.4 ```folder iam```
Sub-commands to manage access policies


##### 4.4.1 ```folder iam add```
Bind an identity to a role to grant permissions

Description: 

Bind an identity to a role on a folder to grant permissions. Role can be 
   'folder.edit' or 'folder.view'

    Example: 
       vke folder iam add folder1 -s user1@account.local \
              -r folder.edit
       vke folder iam add folder1 -s group1 -r folder.view

Flags: 
```
--subject value, -s value	name of the identity to bind (user or group)
--role value, -r value	name of the role to bind
```
##### 4.4.2 ```folder iam export```
Export the direct access policy to a file

Description: 


    Example: 
      vke folder iam export folder1 -o output.txt 

Flags: 
```
--output value, -o value	Output file path
```
##### 4.4.3 ```folder iam import```
Import an access policy from a file

Description: 


    Example: 
      vke folder iam import folder1 -i input.txt

Flags: 
```
--input value, -i value	Input file path
```
##### 4.4.4 ```folder iam remove```
Remove an identity from a role binding

Description: 

Remove an identity from a role binding on a folder. Role can be 
   'folder.edit', 'folder.view' or use '*' to remove all existing roles

    Example: 
       vke folder iam remove folder1 -s user1@account.local \
              -r folder.edit 
       vke folder iam remove folder1 -s group1 -r folder.view

Flags: 
```
--subject value, -s value	name of the identity to bind (user or group)
--role value, -r value	name of the role to bind
```
##### 4.4.5 ```folder iam show```
Show the direct and inherited access policies for the folder

Description: 


    Example: 
      vke folder iam show folder1 


#### 4.5 ```folder list```
List all folders

Description: 

Lists all folders in an organization

    Example: 
       vke folder list 


#### 4.6 ```folder set```
Set the current working folder

Description: 

Set the current folder used for all vke commands that needs folder

    Example: 
       vke folder set folder1 


#### 4.7 ```folder show```
Show information about a folder

Description: 

Shows details of a particular folder

    Example: 
       vke folder show folder1 


#### 4.8 ```folder unset```
Unset the current folder

Description: 

Removes the current folder set in the config file

    Example: 
       vke folder unset 


### 5 ```help```
Shows a list of commands or help for one command


### 6 ```iam```
Commands to manage Identity and Access Management (IAM)


#### 6.1 ```iam group```
Commands to manage groups


##### 6.1.1 ```iam group create```
Create a new group

Description: 

Only organization administrators can create a new group.

    Example:
      vke group create group1 --description 'Purpose of this group' 

Flags: 
```
--description value, -d value	Description for group
```
##### 6.1.2 ```iam group delete```
Delete a group

Description: 

Only organization administrators can delete a group.

    Example:
      vke group delete group1

##### 6.1.3 ```iam group list```
List all groups


##### 6.1.4 ```iam group member```
Commands to manage group members


###### 6.1.4.1 ```iam group member add```
Add a member to a group

Description: 

Add a member to a group. Only organization administrators can create new group.

    Example:
      vke group member add group1 --member-name Bob 

Flags: 
```
--member-name value, -m value	member name (user or group)
```
###### 6.1.4.2 ```iam group member list```
List all members in a group

Description: 

List all members in a group.

    Example:
      vke group member list group1 


###### 6.1.4.3 ```iam group member remove```
Remove a member from a group

Description: 

Remove a member from a group. Only organization administrators can create new group.

    Example:
      vke group member remove group1 --member-name Bob 

Flags: 
```
--member-name value, -m value	member name (user or group)
```
##### 6.1.5 ```iam group show```
Show detailed group information

Description: 


    Example:
      vke group show group1

#### 6.2 ```iam role```
Manage Identity and Access Management (IAM) roles


##### 6.2.1 ```iam role list```
List role definitions

Description: 

List role definitions

#### 6.3 ```iam user```
Commands to manage users


##### 6.3.1 ```iam user list```
List all users


##### 6.3.2 ```iam user show```
Show detailed user info

Description: 


    Example: 
      vke user show user-name@organization


### 7 ```info```
Commands to get information


#### 7.1 ```info region```
Commands to manage regions


##### 7.1.1 ```info region list```
List all regions


### 8 ```organization```
Commands to manage organizations


#### 8.1 ```organization iam```
Sub-commands to manage access policies


##### 8.1.1 ```organization iam add```
Bind an identity to a role to grant permissions

Description: 

Bind an identity to a role on a organization to grant permissions. Role can be 
   'organization.edit' or 'organization.view'

    Example: 
       vke organization iam add fd2c1d78-9f00-4e30-8268-4ab8162080d \
                -s user1@account.local -r organization.edit
       vke organization iam add fd2c1d78-9f00-4e30-8268-4ab8162080d \
                -s group1 -r organization.view

Flags: 
```
--subject value, -s value	name of the identity to bind (user or group)
--role value, -r value	name of the role to bind
```
##### 8.1.2 ```organization iam export```
Export the direct access policy to a file

Description: 


    Example: 
       vke org iam export fd2c1d78-9f00-4e30-8268-4ab8162080d -o output.txt 

Flags: 
```
--output value, -o value	Output file path
```
##### 8.1.3 ```organization iam import```
Import an access policy from a file

Description: 


    Example: 
       vke org iam import fd2c1d78-9f00-4e30-8268-4ab8162080d -i input.txt 

Flags: 
```
--input value, -i value	Input file path
```
##### 8.1.4 ```organization iam remove```
Remove an identity from a role binding

Description: 

Remove an identity from a role binding on a organization. Role can be 
   'organization.edit' or 'organization.view'

    Example: 
       vke organization iam remove fd2c1d78-9f00-4e30-8268-4ab8162080d \
                -s user1@account.local -r organization.edit 
       vke organization iam remove fd2c1d78-9f00-4e30-8268-4ab8162080d \
                -s group1 -r organization.view

Flags: 
```
--subject value, -s value	name of the identity to bind (user or group)
--role value, -r value	name of the role to bind
```
##### 8.1.5 ```organization iam show```
Show the access policy for the organization

Description: 


    Example: 
       vke org iam show fd2c1d78-9f00-4e30-8268-4ab8162080d 

#### 8.2 ```organization show```
Show detailed organization information

Description: 

Shows information related to an organization

    Example: 
       vke org show fd2c1d78-9f00-4e30-8268-4ab8162080d


### 9 ```project```
Commands to manage global projects


#### 9.1 ```project create```
Create new project within a folder

Description: 

Create new project

    Example:
       vke project create project1

Flags: 
```
--display-name value, -d value	sets the display name of the project
--folder value, -f value	specify the current working folder
```
#### 9.2 ```project delete```
Delete project

Description: 

Delete an existing project

    Example:
       vke project delete project1

Flags: 
```
--folder value, -f value	specify the current working folder
```
#### 9.3 ```project get```
Show the current working project

Description: 

Show current project in use for vke

    Example:
       vke project get 


#### 9.4 ```project iam```
Sub-commands to manage access policies


##### 9.4.1 ```project iam add```
Bind an identity to a role to grant permissions

Description: 

Bind an identity to a role on a project to grant permissions. Role can be 
   'project.edit' or 'project.view'

    Example: 
       vke project iam add project1 -s user1@account.local -r project.edit
       vke project iam add project1 -s group1 -r project.view

Flags: 
```
--subject value, -s value	name of the identity to bind (user or group)
--role value, -r value	name of the role to bind
--folder value, -f value	specify the current working folder
```
##### 9.4.2 ```project iam export```
Export the direct access policy to a file

Description: 


    Example: 
      vke project iam export project1 -o file.txt 

Flags: 
```
--output value, -o value	Output file path
--folder value, -f value	specify the current working folder
```
##### 9.4.3 ```project iam import```
Import an access policy from a file

Description: 


    Example: 
      vke project iam import project1 -i file.txt 

Flags: 
```
--input value, -i value	Input file path
--folder value, -f value	specify the current working folder
```
##### 9.4.4 ```project iam remove```
Remove an identity from a role binding

Description: 

Remove an identity from a role binding on a project. Role can be 
   'project.edit', 'project.view' or use '*' to remove all existing roles

    Example: 
       vke project iam remove project1 -s user1@account.local -r project.edit 
       vke project iam remove project1 -s group1 -r project.view

Flags: 
```
--subject value, -s value	name of the identity to bind (user or group)
--role value, -r value	name of the role to bind
--folder value, -f value	specify the current working folder
```
##### 9.4.5 ```project iam show```
Show the direct and inherited access policies for the project

Description: 


    Example: 
      vke project iam show project1 

Flags: 
```
--folder value, -f value	specify the current working folder
```
#### 9.5 ```project list```
List all projects

Description: 

List all project within a folder

    Example:
       vke project list

Flags: 
```
--folder value, -f value	specify the current working folder
```
#### 9.6 ```project set```
Set the current working project

Description: 

Set the current project used for all vke 

    Example:
       vke project set project1 


Flags: 
```
--folder value, -f value	specify the current working folder
```
#### 9.7 ```project show```
Show information about project

Description: 

Show project info

    Example:
       vke project show project1

Flags: 
```
--folder value, -f value	specify the current working folder
```
#### 9.8 ```project unset```
Unset the current project

Description: 

Unset the current project set in the config file

    Example: 
       vke project unset 


Global Flags:
```
--non-interactive, -n	trigger for non-interactive mode (scripting)
--log-file value, -l value	write logging information into a logfile at the specified path
--output value, -o value	the output format can be any of the following values ["json"]
--detail, -d	print the current target, user, Organization and project
--help, -h	show help
--version, -v	print the version
```
