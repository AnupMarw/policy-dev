# AAP - Inventories, credentials, and ad-hoc commands

## Objective

This exercise will cover

- Locating and understanding:
  - Ansible Automation Controller **Inventory**
  - Ansible Automation Controller **Credentials**
- Running ad hoc commands via the Ansible Automation Platform web UI


## Prerequisites

### Set up Git repository

Ansible Automation Platform (AAP), requires your code be stored in version control. We are going to create a GitHub repository for our Ansible playbooks.



#### Create a new Repository in your personal GitHub Account.

Inside the Windows VM complete the following steps.

1. Log in or Create a new account [GitHub](https://github.com/) account
2. Click New Repository
3. Name the reposistory `ansible-working`
4. Check the `Add a README file` checkbox
5. Click the `Create Repository` button
6. In the new repository click the `code` button to expose the `https url` for the repository
7. Click the copy button to copy the `https url` for the repo to use in the next step.


#### Open the newly created repository in VS Code

1. Launch a new VS Code Window.
2. Select the Source Control Tab from the toolbar on the left
3. In the top of the VS Code window click the search bar.
4. Type: `> clone` and choose `Git: Clone`
5. Paste the URL to newly created Repo
6. In the choose a folder dialog, select your `repos` folder.
7. Click the `select as Repository Destination` button
8. In the Visual Studio Code dialog click the `Open in this window` or `Add to Workspace` button to open the repository in VS Code
9. In the left Toolbar click the Explorer button.


### Log into AAP

Access the Dashboard at the following URL https://52.53.214.20


Log into the dashboard with the username and the password from the spreadsheet


You should see something similar to the screenshot below.

![image-20220222024405897](images/image-20220222024405897.png)


## Create an Inventory

Let’s get started: The first thing we need is an inventory of managed hosts. This is the equivalent of an inventory file in Ansible Engine. There is a lot more to it (like dynamic inventories) but let’s start with the basics.

In the web UI menu on the left side, go to **Resources** → **Inventories**, click the **Add** button and choose **Add inventory**

Provide the following:

* **Name**:  First Inventory-[your initials]
* **Description**: My first inventory file
* **Organization**: Default

Click **Save**

At the top of the page click the **Hosts** button, the list will be empty since we have not added any hosts yet.



Let's add our hosts.  


Click the **Add** button and give a **Name**, and **Description**: 

* **Name**: Server-[your initials]

* **Description**: Node from the spreadsheet

* Under **Variables** confirm **YAML** is highlighted and then paste the following:

  ```yaml
  ansible_host: <IP of node from spreadsheet> 
  ```

  

* Click **Save** 

You have now created an inventory with new managed host.



## Machine Credentials

One of the great features of the Ansible Automation Platform is to make credentials usable to users without making them visible. To allow AAP to execute jobs on remote hosts, you must configure connection credentials.

> **TIP**:This is one of the most important features of Automation Platform: **Credential Separation**! Credentials are defined separately and not with the hosts or inventory settings.

We need to configure the Ansible Automation Platform with the Controller SSH Private Key to enable it to connect to our managed nodes.



Copy the **complete private key** (including “BEGIN” and “END” lines) from [here](https://gist.github.com/jruels/00b5e617f4f60e5bc692ae8450089a07)



Now configure the credentials to access the managed hosts from Ansible Automation Platform.

In the **Resources** menu choose **Credentials**, and click **Add** then fill in the following:

* **Name**: Linux credentials-[your initials]
* **Description**: Credentials to authenticate over SSH
* **Organization**: Default
* **Credential Type**: Machine

Under **Type Details** fill in: 

* **Username**: ansible

* **SSH Private Key**: Paste the private key from above.  

**Privilege Escalation Method**: sudo 

> **TIP**: Whenever you see a magnifying glass icon next to an input field, clicking it will open a list to choose from.

* Click **Save**

Go back to the **Resources -> Credentials -> Linux credentials-[your initials]** and note that the SSH key is not visible.

You have now set up credentials for Ansible to access your managed host.



## Run Ad Hoc Commands

Ansible can run ad hoc commands from AAP as well.

In the web UI go to **Resources → Inventories → First Inventory-[your initials]**

- Click **Hosts** at the top of the page to change into the hosts view.
- Click **Run Command**.
- On the next screen specify the ad-hoc command: 
- **Module**: choose `ping`
- Click **Next**
- **Execution Environment**: Default execution environment
- Click **Next**
- **Machine Credential**: Linux credentials[your initials]
- Click **Next**
- Click **Launch**, and watch the output. 



The simple **ping** module doesn’t need options. For other modules, you need to supply the command to run as an argument. Try the **command** module to find the user ID of the executing user using an ad-hoc command.

- **Module**: command
- **Arguments**: id

> **TIP**: After choosing the module to run, Tower will provide a link to the docs page for the module when clicking the question mark next to "Arguments". This is handy, give it a try.



## Challenge Lab: Ad Hoc Commands

Run an ad-hoc command to make sure the package `tmux` is installed on the host. If unsure, consult the documentation either via the web UI as shown above or by running `ansible-doc yum` on your AAP control host.



The instructor will provide the solution. 



## Congrats!
