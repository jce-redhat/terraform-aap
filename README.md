# terraform-aap - Demo for installing and configuring AAP using Terraform and IaC

## Quick start

Before you begin, you will need the following information:

* An offline token for downloading the AAP installation bundle.  One can be generated on the [Red Hat Customer Portal](https://access.redhat.com/management/api/).
* An Automation Hub token for downloading content from the Red Hat Automation Hub.  One can be generated on the [Automation Hub](https://console.redhat.com/ansible/automation-hub/token)
* An AWS access key and secret key, used by terraform to create the AAP infrastructure on AWS.

### Set up prerequisites

1. Clone the repo and `cd` into the repo directory

2. Create an Ansible vault file
   * `cp vars/vault.yml.example vars/vault.yml`
   * `ansible-vault encrypt vars/vault.yml`
   * `ansible-vault edit vars/vault.yml`, and modify the vault variable defaults per the comments in the file

3. Create the vars/terraform\_aap.yml file
   * `cp vars/terraform_aap.yml.example vars/terraform_aap.yml`
   * Change the 'terraform\_build\_name' variable.  This build name will be used when cloning the terraform infra repo.
   * Change the 'terraform\_aws\_region' variable if a different AWS region is desired.
   * Change the 'terraform\_state\_aws\_bucket' variable to a unique S3 bucket name.  This bucket will be created in the AWS region specified.

4. Create the vars/aap\_setup.yml file
   * `cp vars/aap_setup.yml.example vars/aap_setup.yml`

5. Set the AWS-related shell environment variables
   * `export AWS_ACCESS_KEY_ID="my-aws-access-key"`
   * `export AWS_SECRET_ACCESS_KEY="my-aws-secret-key"`

6. Set the Automation Hub shell environment variable
   * `export ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_TOKEN="automation-hub-token"`
   * Token can be generated at https://console.redhat.com/ansible/automation-hub/token

7. `ansible-galaxy collection install -r collections/requirements.yml`

8. `ansible-galaxy role install -r roles/requirements.yml`

9. Install the python boto3 requirement for amazon.aws
   `pip3 install --user boto3`

10. Install terraform
   * If on RHEL 8 or above, `ansible-playbook install-terraform.yml`
   * If not, see https://developer.hashicorp.com/terraform/downloads

### Create AAP infrastructure

1. `ansible-playbook terraform-aap-on-aws.yml`

2. Optionally, `ansible-playbook stig-controllers.yml` to apply the RHEL STIG to the controller node(s)

3. `ansible-playbook install-aap.yml --ask-vault-pass`

## The Details

*Work in progress*

