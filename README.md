# Terraform Beginner Bootcamp 2023

## Semantic Versioning  :mage:

This project is going to use semantic versioning for its tagging. [semver.org](https://semver.org/)

The general format:

 **MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes 

# Installing the terraform CLI


### Considerations with the Terraform CLI changes 
The gpg keyring changes have affected the Terraform CLI installation process. We need to follow the updated instructions from the Terraform Documentation and modify our installation script accordingly.
 - [**Install Terraform**](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)


### Considerations for Linux Distribution 

This Project is built against Ubuntu.
Please consider checking your Linux Distribution and change to accordingly to your distribution needs.

- [**How to check linux OS Version**](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

An example of checking os version:
```sh
$ cat /etc/os-release 
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```




### Refactoring into Bash Scripts 

While fixing Terraform CLI gpg depreciation issues, we noticed that the bash script steps had a lot of code we hadn't previously seen. As a result, we decided to create a Terraform CLI installation [**script**](./bin/install_terraform_cli).

- This will keep the [**.gitpod.yml**](./.gitpod.yml) Task File tidy.
- This will allow us have an easier way to debug and excute a muanually Terraform CLI Installation.
- This will allow better portability for other projects that need to install Terraform CLI.



### Shebang 
A [**Shebang**](https://en.wikipedia.org/wiki/Shebang_(Unix)) (*pronounced Sha-bang*) tells the bash script 
what program will interpret the script e.g  `#!/bin/bash`.

ChatGPT recommended this format for bash: `#!/usr/bin/env bash`.

- for portability on different OS distributions
- will search the user's PATH for the bash executable



#### Execution Considerations
When executing the bash script we can use the `./` shorthand notation to execute the bash script.

e.g `./bin/install_terraform_cli`

If we are using a script in .gitpod.yml we need to point the script to a program to interpret it.

e.g `source ./bin/install_terraform_cli`

#### Linux Permissions Considerations

In order to make our bash scripts executable we need to change linux permissions for the file to be executable at the user mode.

```sh
chmod u+x ./bin/install_terraform_cli
```

Alternatively:
```sh
chmod 744 ./bin/install_terraform_cli
```

- [Linux permissions CHMOD](https://en.wikipedia.org/wiki/Chmod)



### Gitpod Lifecycle (Before, Init, Command)

We need to be careful we using the init because it will not rerun if we restart an existing workspace.

- [Gitpod Tasks Configuration](https://www.gitpod.io/docs/configure/workspaces/tasks)


#### Working with Env Vars

We can list outv all Environment Variables (Env Vars) using the `env` command

We can filter specific env vars using grep eg. `env | grep AWS_`

#### Setting, Unsetting and Printing Env Vars

In the terminal we can set Env Vars using `export HELLO='World'`

In the terminal we can print out the set Env Vars using echo eg. `echo $HELLO`

we can and env var temporarily when running a command
```sh
HELLO='world' ./bin/print_message
```

In the terminal we can unset Env Vars using `unset HELLO`

Within a bash script we set an env without writing export eg.

```sh
#!/usr/bin/env bash

HELLO='world'

echo $HELLO
```

#### Scoping of Env Vars 

when you  open up new window for bash, It wil not be aware of env vars that you hav set in another bash window.

If you want the Env Vars to persist across all future bash terminals that are open you need to set env vars in your bash profile. e.g `.bash_profle`

#### Persisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage

```
gp env HELLO='world'
```
All future workspaces  launched will set the envs vars for all bash terminals opened in those workspaces.

You can also set env vars in the `.gitpod.yml` but this can only contain non-sensitive env.

### AWS CLI Installation

AWS CLI is installed for this project via this [**script**](./bin/install_aws_cli)

- [**Getting Started Install (AWS CLI)**](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

- [**AWS CLI Env Vars**](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials are configured correctly by running this AWS CLI command:
```sh
aws sts get-caller-identity
```

If it is successful you should see a json payload return that looks like this:

```json
{
    "UserId": "AIDASAMPLEUSERID",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/DevAdmin"
}
```

we'll need to generate AWS CLI credentials from IAM User in order to use the AWS CLI.

## Terraform Basics

### Terraform Registry

Terraform sources their providers and modules from the Terraform registry which located at [registry. terraform.io](https://registry.terraform.io/)

- **Providers** are interfaces to APIs that allow creating resources in Terraform.

- **Modules** are a way to make Terraform code modular, portable and sharable.

[Random Terraform Provider](https://registry.terraform.io/providers/hashicorp/random)

### Terraform console

We can see a list of all the Terrform commands by simply typing `terraform` in the terminal

#### Terraform Init

To begin a new terraform project, we need to run `terraform init` which will fetch the binaries for the terraform providers that our project requires.


#### Terraform Plan
This will generate out a changeset, about the state of our infrastructure and what will be changed.


We can output this changeset ie. "plan" to be passed to an apply, but often you can just ignore outputting.

#### Terraform Apply

`terraform apply`

When you run the `terraform apply` command, it will execute the changeset generated by the plan. The command will prompt you to confirm if you want to proceed with the plan. You can then enter 'yes' or 'no' accordingly.

To automatically approve a `terraform apply`, you can make use of the `-auto-approve` flag eg. `terraform apply --auto-approve`


#### Terraform Destroy

The `terraform destroy` command is used to tear down and destroy the infrastructure resources managed by Terraform. It's essentially the opposite of terraform apply, which creates or updates resources based on your Terraform configuration.

Again to automatically approve a `terraform destroy`, you can make use of the `-auto-approve` flag eg. `terraform destroy --auto-approve`

### Terraform Lock Files

`.terraform.lock.hcl` is a file that locks the version of providers or modules used in a project.

This file  **should be committed** to your Version Control System (VSC) eg. Github

### Terraform State Files

`.terraform.tfstate` is a file that stores the current state of your infrastructure in JSON format. It contains detailed information about the resources created by Terraform in your infrastructure, such as the resource types, attributes, and dependencies. This file is automatically generated and updated by Terraform each time you apply changes to your infrastructure. 

The `.terraform.tfstate` file is an essential part of Terraform's state management system, as it enables Terraform to compare the current state of your infrastructure with the desired state declared in your configuration files.

This file **should not be commited** to your VCS.

This file can contain sensentive data.

If you lose this file, you lose knowning the state of your infrastructure.

`.terraform.tfstate.backup` is the backup file of the previous state file. This file is created each time you run `terraform apply` or `terraform destroy` commands, and it contains the previous state of your infrastructure before the latest changes were applied. The `.terraform.tfstate.backup` file can be used to restore the previous state of your infrastructure in case of any issues or errors that may have occurred during the latest `terraform apply` or `terraform destroy` command. It is recommended to keep this file along with the current state file, `.terraform.tfstate`, so that you can easily switch between the two if needed.

### Terraform Directory

The `.terraform` directory is an important directory in a Terraform project, as it typically contains the downloaded binaries of Terraform providers. These providers are used by Terraform to interact with various infrastructure platforms and services. When you run `terraform init`, Terraform downloads the providers specified in your configuration files and places them in the `.terraform` directory. 

It's **important to note** that the `.terraform` directory should not be manually edited or modified, as doing so could cause issues with your Terraform project. If you need to modify a provider, you should update the provider configuration in your Terraform files and then run `terraform init` again to update the provider binaries in the `.terraform` directory.