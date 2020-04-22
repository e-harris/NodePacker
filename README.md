# Nodejs packer

This readme will give you the appropriate instructions to create your own AMI on AWS using packer.

If you do not have a cookbook, feel free to use ours:

https://github.com/e-harris/NodeCookbook

### Packer

Packer uses a .json file to create an image on AWS, also known as an AMI. These images can be used to create EC2 instances.

## Pre-requisites
 - AWS access and secret access keys
 - AWS subnet
 - Packer v1.1.5
 - ChefDK version: 4.7.73
 - Chef Infra Client version: 15.7.32
 - Chef InSpec version: 4.18.51
 - Test Kitchen version: 2.3.4
 - Foodcritic version: 16.2.0
 - Cookstyle version: 5.20.0


## Creating the AMI

#### In the root directory:

### Getting the cookbook

You will need to edit the berksfile.

`cookbook` refers to the name of your cookbook

`git:` refers to the link of the online repo

Once you have entered this information, run `berks vendor`
 - You can supply a name after this if you wish for the directory to be called something else.

### packer.json

Add your information to the packer.json supplied by this repo. Below will cover the bits of information that's missing and what is does.

##### Variables

These are your AWS access and secret access keys. For security, environment variables have been used to stop unwanted people obtaining these keys. Our variables are called `AWS_ACCESS_KEY_ID` and `AWS_ACCESS_KEY_ID` respectively.

##### Builders 

`"subnet_id":` Each AMI is associated with a given subnet specified here.

```
"ssh_keypair_name": "",
"ssh_private_key_file": "",
```
The keypair name is the name of the file in which your ssh key is saved as. The private key file is the path to this file, not directory. **ONLY** the private key file needs to end in `.pem`.

`"ami_name":` This is the name your AMI will be stored under on AWS.

##### Provisioners

`"cookbook_paths":` This is the path to your cookbook that was created by running `berks vendor`.

`"run_list": ["node::default"]` This is the name of your cookbook. Ours was called `node`. If you are using your own cookbook, replace this with the name of your cookbook.

### Making the AMI

Run `packer validate packer.json` to validate packer.json.

Now run `packer build packer.json` and this will start the creation of the AMI and will run the provisioners of the cookbook.


Hey Presto! You have now made your AMI! Victory :taco:!
