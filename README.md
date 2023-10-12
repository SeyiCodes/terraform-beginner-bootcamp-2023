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

