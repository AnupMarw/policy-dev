# Lab 11: Authenticating to HCP Terraform from the CLI
## Logging in and Secure CLI Authentication

---

### Overview

In this lab, you will learn how to securely authenticate your local Terraform CLI with HCP Terraform using the `terraform login` command. You’ll follow best practices for generating and managing API tokens, and understand how this authentication enables secure remote operations, state management, and collaboration in HCP Terraform.

---

### Context

HCP Terraform runs Terraform operations and stores state remotely, providing a secure and reliable environment for infrastructure management. To use HCP Terraform from your local CLI, you must first authenticate by logging in and generating an API token. This process links your CLI to your HCP Terraform account, allowing you to trigger remote plans, applies, and other operations directly from your terminal.

---

### Hands-On Tasks

#### 0. Clone the Example Terraform Configuration

For this lab, you will use a sample Terraform configuration that demonstrates variable usage. Clone the repository to your local machine and change into the directory:

```sh
git clone https://github.com/jruels/learn-terraform-variables.git
cd learn-terraform-variables
```

All subsequent commands in this lab should be run from within the `learn-terraform-variables` directory.

#### 1. Prerequisites

- Terraform CLI (version 1.1.0+) installed locally
- An HCP Terraform account

#### 2. Start the Login Flow

Run the following command in your terminal to begin the authentication process:

```sh
terraform login
```

You will be prompted to confirm that you want to authenticate. Type `yes` and press Enter.

#### 3. Generate an API Token

A browser window will open to the HCP Terraform login screen. Enter a token name (or use the default), then click **Create API token** to generate your authentication token.

If your browser does not open automatically, copy and paste the provided URL from your terminal into your browser.

#### 4. Add the Token to the CLI

Copy the generated token and paste it into your terminal when prompted:

```
Token for app.terraform.io:
  Enter a value: <paste your token here>
```

Terraform will store the token in plain text in the following file for use by subsequent commands:

```
~/.terraform.d/credentials.tfrc.json
```

#### 5. Verify Authentication

After entering your token, you should see a message similar to:

```
Retrieved token for user <your-username>
Welcome to HCP Terraform!
```

You are now authenticated and ready to perform remote operations with HCP Terraform.

---

### Expected Results

- Your local Terraform CLI is securely authenticated with HCP Terraform.
- The API token is stored in your credentials file for future use.
- You can now trigger remote plans, applies, and manage state in HCP Terraform from your terminal.

---

### Reflection & Challenge

- **Reflection:** Why is it important to use API tokens and secure authentication when working with cloud-based infrastructure tools?
- **Challenge:**
  - Explore the `~/.terraform.d/credentials.tfrc.json` file and consider how you would rotate or revoke tokens if needed.

---

You have now learned how to securely authenticate your Terraform CLI with HCP Terraform, enabling safe and collaborative remote infrastructure management. 

---

### Creating a Workspace in HCP Terraform (CLI-Driven Workflow)

#### Overview

Workspaces in HCP Terraform are used to organize and manage collections of infrastructure resources. Each workspace contains its own state, variables, and configuration, allowing you to separate environments, projects, or components for better collaboration and control. Creating a workspace using the CLI-driven workflow ensures your local configuration is linked to a remote workspace for centralized state management and team access.

---

#### 1. Update Your Configuration with a Cloud Block

Edit your `main.tf` file to include a `cloud` block specifying your HCP Terraform organization and the desired workspace name. This tells Terraform where to create and manage your workspace.

Example:

```hcl
terraform {
  cloud {
    organization = "your-organization"
    workspaces {
      name = "your-workspace"
    }
  }
  required_providers {
    aws = {
      source  = "hashicorp/aws"
    }
  }
}
```

---

#### 2. Initialize the Workspace

Run the following command in your project directory:

```sh
terraform init
```

Terraform will initialize the working directory, download required provider plugins, and create the new workspace in your HCP Terraform organization if it does not already exist. You should see confirmation that HCP Terraform has been successfully initialized.

---

#### 3. Assign Variable Sets to the Workspace

If you are not using a global variable set, assign the appropriate variable set (such as AWS credentials) to your new workspace in the HCP Terraform UI:
- Navigate to your workspace in the UI.
- Go to the **Variables** page.
- Under **Variable sets**, click **Apply variable set** and select the relevant set.

This ensures your workspace has access to the necessary credentials and configuration values for provisioning resources.

---

#### Expected Results

- A new workspace is created in your HCP Terraform organization, linked to your local configuration.
- The workspace is initialized and ready for remote operations.
- Variable sets are assigned, enabling secure and consistent infrastructure management.

---

#### Reflection & Challenge

- **Reflection:** How does using workspaces and variable sets in HCP Terraform improve collaboration and infrastructure management compared to local-only workflows?
- **Challenge:**
  - Try creating a second workspace for a different environment (e.g., staging or dev) and observe how state and variables are isolated.
  - Experiment with assigning different variable sets to different workspaces and note the impact on your Terraform runs.

---

### Configuring Workspace Variables and Provisioning Infrastructure

#### 1. Configure Workspace-Specific Variables

HCP Terraform allows you to define input variables directly in the workspace UI, making it easy to customize your infrastructure without editing code. To add or modify variables:
- Navigate to the **Variables** page for your workspace in the HCP Terraform UI.
- In the **Workspace variables** section, click **+ Add variable**.
- Select the **Terraform variable** radio button.
- Set the key to `instance_type` and the value to `t2.micro`, then click **Add variable**.
- Click **+ Add variable** again, set the key to `instance_name` and the value to `Provisioned by Terraform`, then click **Add variable**.

Your workspace is now configured with these input variables, which will be used in your Terraform configuration.

---

#### 2. Apply Planned Changes

With your variables set, you can now provision your infrastructure using the CLI-driven workflow. In your project directory, run:

```sh
terraform apply
```

Terraform will trigger a run in HCP Terraform and stream the output to your terminal. You will see a summary of the planned changes and a prompt to confirm before applying them. You can also view and manage the run in the HCP Terraform UI by following the run URL provided in your command output.

Terraform will not modify your infrastructure until you confirm and apply the plan. This checkpoint allows you and your team to review the proposed changes before they are made.

---

#### 3. View Run Details, Resources, and Outputs

After the apply completes, return to your workspace's **Overview** page in the HCP Terraform UI. Here you can:
- Review details about the latest run, including resource changes and outputs.
- View a table of all resources currently managed in the workspace.
- Access the **Outputs** tab to see any outputs defined in your configuration, such as resource IDs or IP addresses.

This centralized visibility makes it easy to monitor, audit, and share information about your deployed infrastructure.

---

#### Summary

- You configured workspace-specific variables in the HCP Terraform UI.
- You used `terraform apply` to provision infrastructure, reviewing and confirming planned changes.
- You viewed run details, managed resources, and accessed outputs in the workspace UI.

Your workspace is now fully configured and managing infrastructure with HCP Terraform.